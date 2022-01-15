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


## 3. dialog 구현
