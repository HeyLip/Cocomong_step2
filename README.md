# cocomong_step2

> 이번 cocomong_step2에서 구현하고자 하는 것은 음식물들을 List로 나타내는 home page를 만드는 것입니다.   
> 최종결과는 다음과 같습니다.   
> ![Screenshot_16422626601](https://user-images.githubusercontent.com/63987139/149629772-127456db-598a-4502-8140-0a0d39978989.png)   
> 시작하기에 앞서 cocomong_step1을 완료하지 못하셨으면 아래 링크를 참고하세요.   
> - Link: [cocomong_step1](https://github.com/HeyLip/Cocomong-step1)

## 1. UI 구현
_HomePageState 클래스 밑에 build라는 Widget을 아래와 같이 구현을 합니다.   
<pre>
<code>
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('나의 냉장고'),
        centerTitle: true,
        leading: IconButton(
          icon: const Icon(Icons.person),
          onPressed: () async {
            //버튼을 누를 시 어떤 기능이 작동하는지 구현하는 곳
          },
        ),
      ),

      floatingActionButton: SizedBox(
        width: 350,
        height: 70,
        child: FloatingActionButton.extended(
          backgroundColor: const Color(0xFF5B836A),
          onPressed: () {
          },
          label: const Text(
            '새 음식 추가',
            style: TextStyle(
              color: Colors.white,
              fontWeight: FontWeight.bold,
              fontSize: 20,
            ),
          ),
          icon: const Icon(
            Icons.add,
            size: 28,
            color: Colors.white,
          ),
          shape: const RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(15.0)),
          ),
        ),
      ),
      floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat,
    );
  }
</code>
</pre>

---------------------------------------------------------------------------------

* appbar   

세부적으로 설명을 드리면 아래의 코드는 appbar를 구현하는 코드입니다.
<pre>
<code>
appBar: AppBar(
        title: const Text('나의 냉장고'),
        centerTitle: true,
        leading: IconButton(
          icon: const Icon(Icons.person),
          onPressed: () async {
            //버튼을 누를 시 어떤 기능이 작동하는지 구현하는 곳
          },
        ),
      ),
</code>
</pre>
![Screenshot_16422645511](https://user-images.githubusercontent.com/63987139/149629833-84d7bbca-318d-413c-ae7b-012eb4883fb7.png)   
 
위 이미지와 같은 결과가 출력이 됩니다.

-------------------------------------------------------------------------------------------

* FloatingActionButton      

다음 코드는 밑에 버튼을 구현하는 코드입니다. 이러한 버튼은 FloatingActionButton이라고 부릅니다.
<pre>
<code>
floatingActionButton: SizedBox(
        width: 350,
        height: 70,
        child: FloatingActionButton.extended(
          backgroundColor: const Color(0xFF5B836A),
          onPressed: () {
            //버튼을 누를 시 어떤 기능이 작동하는지 구현하는 곳
          },
          label: const Text(
            '새 음식 추가',
            style: TextStyle(
              color: Colors.white,
              fontWeight: FontWeight.bold,
              fontSize: 20,
            ),
          ),
          icon: const Icon(
            Icons.add,
            size: 28,
            color: Colors.white,
          ),
          shape: const RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(15.0)),
          ),
        ),
      ),
      floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat,
</code>
</pre>

![Screenshot_1642264551](https://user-images.githubusercontent.com/63987139/149629959-4cbb285d-7a7d-43de-be42-ef584fd681ba.png)

위 이미지와 같은 결과가 출력이 됩니다.


## 2. firebase 연결

_HomePageState 클래스에 다음과 같은 코드를 작성하여 줍니다. 이 코드는 Firebase에 어떤 부분에서 data를 읽어올지 초기화해주는 코드입니다.   
<pre>
<code>
late CollectionReference database;

 @override
  void initState() {
    super.initState();
    database = FirebaseFirestore.instance
        .collection('user')
        .doc(widget.user!.uid)
        .collection('Product');
  }
</code>
</pre>   



앞서 구현한 AppBar와 FloatingActionButton 사이에 아래 코드를 작성하여 줍니다.   
이 부분은 Firebase로부터 Data를 직접 읽어드려와 출력해주는 Page입니다.

<pre>
<code>
body: Column(
        children: <Widget>[
          Expanded(
            child: StreamBuilder<QuerySnapshot>(
              stream: database.orderBy('expirationDate', descending: false).snapshots(),
              builder: (BuildContext context,
                  AsyncSnapshot<QuerySnapshot> snapshot) {
                if (snapshot.hasError) {
                  return Center(
                    child: Text('Error: ${snapshot.error}'),
                  );
                }
                switch (snapshot.connectionState) {
                  case ConnectionState.waiting:
                    return const Center(
                      child: Text('Loading...'),
                    );
                  default:
                    return ListView(
                        padding: const EdgeInsets.all(16.0),
                        children:
                        _buildListCards(context, snapshot) // Changed code
                    );
                }
              },
            ),
          ),
        ],
      ),
</code>
</pre>




## 3. dialog 구현

각 음식들을 Tap할 때, 음식의 상세한 정보들이 나오는 것을 구현하는 코드입니다.   
<pre>
<code>
void dialog(
      String photoURL, String title, DateTime date, String description) {
    showDialog(
        context: context,
        barrierDismissible: true,
        builder: (BuildContext context) {
          return AlertDialog(
            shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.circular(10.0)),
            title: Row(
              children: <Widget>[
                SizedBox(
                  width: 70,
                  height: 70,
                  child: ClipOval(
                    child: Image.network(photoURL, fit: BoxFit.fill),
                  ),
                ),
                SizedBox(
                  width: 194,
                  child: Column(
                    children: <Widget>[
                      Text(
                        title,
                        style: const TextStyle(fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(
                        height: 5,
                      ),
                      Text(
                        '${DateFormat('yyyy').format(date)}.${date.month}.${date.day} 까지',
                        style: const TextStyle(
                            fontWeight: FontWeight.bold, fontSize: 14),
                      ),
                    ],
                  ),
                ),
              ],
            ),
            content: Column(
              mainAxisSize: MainAxisSize.min,
              crossAxisAlignment: CrossAxisAlignment.start,
              children: <Widget>[
                Text(
                  description,
                ),
              ],
            ),
            actions: <Widget>[
              SizedBox(
                width: 80,
                height: 50,
                child: ElevatedButton(
                  style: ElevatedButton.styleFrom(
                      primary: const Color(0xFF5B836A),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(15.0),
                      )
                  ),
                  child: const Text("확인"),
                  onPressed: () {
                    Navigator.pop(context);
                  },
                ),
              ),
            ],
          );
        });
  }
</code>
</pre>

