# Crypto Zombies Lesson 2 좀비가 희생물을 공격하다.





본 게시물은 https://cryptozombies.io/ko 를 참고하여 작성한 문서입니다.







## Chap 2. 매핑과 주소



<img width="661" alt="스크린샷 2020-01-28 오후 4 30 13" src="https://user-images.githubusercontent.com/54495632/73243853-f151a080-41eb-11ea-89ae-0c4a47f259f5.png">



### 주소


<img width="665" alt="스크린샷 2020-01-28 오후 4 30 23" src="https://user-images.githubusercontent.com/54495632/73244417-3f1ad880-41ed-11ea-91d0-9124b5826d87.png">


 이더리움 블록체인은 은행 계좌와 같은 계정들로 이루어져 있다. 계정은 이더리움 블록체인상의 통화인 이더(코인)의 잔액을 가지고 있다. 계정과 계정을 통해 이더를 서로 주고 받을 수 있다.

 각 계정은 은행의 계좌 번호와 같이 주소를 가지고 있다. 주소란 특정 계정을 가리키는 고유 식별자이다. 주소는 특정 유저 혹은 스마트 컨트랙트가 소유한다.

따라서, 주소를 소유권을 나타내는 고유 아이디로 활용할 수 있다. 한 명의 유저가 앱을 통해 새로운 좀비를 생성하면 해당 주소에 생성된 좀비에 대한 소유권을 보유한다.




### 매핑

<img width="672" alt="스크린샷 2020-01-28 오후 4 30 31" src="https://user-images.githubusercontent.com/54495632/73244412-3aeebb00-41ed-11ea-913f-e16941d48c94.png">


매핑이란 솔리디티에서 구조화된 데이터를 저장하는 또 다른 방법이다.

Ex)

```sol
// 금융 앱 용으로 유저의 계좌 잔액을 보유하는 uint 저장
mapping (address => uint) public accountBalance;
// 혹은 userID로 유저 이름을 저장/검색하는데 매핑을 쓸 수도 있다.
mapping (uint =>string) userIdToName;
```

매핑은 기본적으로 key - value 형식으로 저장하며, 데이터(value)를 저장하고 키를 통해 검색한다. 

첫 번째 예시에서 키는 address고 값은 uint 이다.
두 번째 예시에서 키는 uint고 값은 string 이다.



### 직접해보기





<img width="680" alt="스크린샷 2020-01-28 오후 4 30 36" src="https://user-images.githubusercontent.com/54495632/73246615-658f4280-41f2-11ea-8840-b1bd8e87e32b.png">


```sol
pragma solidity ^0.4.19;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        NewZombie(id, _name, _dna);
    } 

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}
```





## Chap 3. Msg.sender

<img width="673" alt="스크린샷 2020-01-28 오후 5 29 17" src="https://user-images.githubusercontent.com/54495632/73247251-d2570c80-41f3-11ea-81e3-2d18180f33b7.png">





### msg.sender

<img width="677" alt="스크린샷 2020-01-28 오후 5 29 29" src="https://user-images.githubusercontent.com/54495632/73247305-ed298100-41f3-11ea-8db5-3c40e64ccf2c.png">


msg.sender는 현재 함수를 호출한 사람(or 스마트 컨트랙트)의 주소를 가리키는 msg.sender가 있다.

- 솔리디티에서 함수 실행은 항상 외부 호출자가 시작한다.
- 컨트랙트는 컨트랙트 함수를 호출할 때 까지 블록체인 상에서 아무것도 안하고 있는 상태로 있는다.


Ex )

```sol
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
// msg.sender에 대해 _myNumber가 저장되도록 favoriteNumber 매핑을 업데이트한다.
favoriteNumber[msg.sender] = _myNumber;
// ^데이터를 저장하는 구문은 배열로 데이터를 저장할 때와 동일하다.
}

function whatIsMyNumber() public view returns (uint){
//sender의 주소에 저장된 값을 불러온다.
//sender가 'setMyNumber'을 아직 호출하지 않았다면 반환값은 '0'이 될 것이다.
return favoriteNumber[msg.sender];
}
```

setMyNumber을 호출하여 본인의 주소와 연결된 우리 컨트랙트 내에 uint를 저장할 수 있다.
msg.sender를 활용하면 이더리움 블록체인의 보안성을 이용할 수 있다. 즉, 타인이 특정 데이터를 변경하려면 해당 이더리움 주소와 관련된 개인 키를 알아야된다.





### 직접 해보기

<img width="676" alt="스크린샷 2020-01-28 오후 5 29 34" src="https://user-images.githubusercontent.com/54495632/73247935-5362d380-41f5-11ea-90d0-5bb2f360d754.png">


1. 새로운 좀비의 id가 반환된 후 zombieToOwner 매핑을 업데이트하여 id에 대하여 msg.sender 가 저장되도록 한다.
2. msg.sender을 고려하여 ownerZombieCount를 증가시킨다.

- 솔리디티에서도 uint 에 대한 ++ 연산이 가능하다. // 1 증가

```sol
uint num=0;
num++;
//num 은 1
```


```sol
pragma solidity ^0.4.19;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id]=msg.sender;
        ownerZombieCount[msg.sender]++;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}
```





## Chap 4. Require

<img width="671" alt="스크린샷 2020-01-28 오후 6 57 26" src="https://user-images.githubusercontent.com/54495632/73253763-7ba3ff80-4200-11ea-913c-7c620fa58632.png">



### Require


require 를 활용하면 특정 조건이 참이 아닐 때 함수가 에러 메세지를 발생하고 실행을 멈춘다.

Ex)

```sol
function sayHiToVitalik(string _name) public returns (string){
require (keccak256(_name) == keccak256("Vitalik"));
// _name 이 "Vitalik" 인지 비교
//참이 아닐 경우 에러메세지 출력 후 함수를 벗어남
// "Vitalik" 을 keccak256 해싱한 값과 _name 변수를 keccak256 해싱한 값이 같은지 비교

//참일 경우
return "Hi!";
//Hi! 를 리턴한다.
}
```


sayHiTovitalik("Vitalik") 로 이 함수를 실행하면 "Hi!" 가 반환된다.
Vitalik 이 아닌 다른 인자를 넣을 경우 에러 메세지가 출력되고 Hi! 가 리턴되지 않는다.

require은 함수 실행 전 참이어야 하는 특정 조건을 확인하는데 있어서 유용하다.



### 직접 해보기


<img width="678" alt="스크린샷 2020-01-28 오후 6 57 31" src="https://user-images.githubusercontent.com/54495632/73255529-a5aaf100-4203-11ea-9867-e86f0b339bf5.png">


require를 이용하여 첫 좀비를 만들 때 createRandomZombie 함수를 유저 당 한 번만 호출되도록 설정한다. (좀비 생성에 제한을 준다.)

```sol
pragma solidity ^0.4.19;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender;
        ownerZombieCount[msg.sender]++;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        require(ownerZombieCount[msg.sender]==0);
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}
```





## Chap 5. 상속




<img width="658" alt="스크린샷 2020-01-28 오후 7 28 14" src="https://user-images.githubusercontent.com/54495632/73255925-60d38a00-4204-11ea-84bb-a961cb30919c.png">



### 상속

솔리디티에선 컨트랙트 간의 상속 기능을 제공한다. (기본적으로 다른 객체지향언어에서의 '상속' 개념과 같은 것 같다)
부분 집합적인 컨트랙트가 있을 때 논리적 상속을 위해 활용할 수 있다.
or 동일한 로직을 다수의 클래스로 분할하여 코드를 정리할 때도 활용할 수 있다.

상속은 is 를 이용하여 선언한다.
contract 컨트랙트1이름 is 상속할컨트랙트2이름 {

}
컨트랙트1은 컨트랙트2를 상속한다. 


Ex)

```sol
contract Doge {
function catchphrase() public returns (string) {
  return "So Wow CryptoDoge";
 }
}

contract BabyDoge is Doge {
 function anotherCatchphrase() public returns (string) {
   return "Such Moon BabyDoge";
 }
}
```

BabyDoge 컨트랙트는 Doge 컨트랙트를 상속한다. 
BabyDoge 컨트랙트에서 본인의 function인 anotherCatchphrase 함수외에 
상속된 클래스의 function인 catchphrase에도 접근이 가능하다.



### 직접 해보기



<img width="650" alt="스크린샷 2020-01-28 오후 7 28 20" src="https://user-images.githubusercontent.com/54495632/73256575-7eedba00-4205-11ea-8b48-03f585de5381.png">


```sol
pragma solidity ^0.4.19;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender;
        ownerZombieCount[msg.sender]++;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        require(ownerZombieCount[msg.sender] == 0);
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}

contract ZombieFeeding is ZombieFactory {
// 해당 부분 ZombieFeeding 컨트랙트에 ZombieFactory 컨트랙트를 상속하였다.
}
```



## Chap 6. Import

<img width="683" alt="스크린샷 2020-01-28 오후 7 39 20" src="https://user-images.githubusercontent.com/54495632/73256796-f28fc700-4205-11ea-9482-f7cac03b765f.png">



### Import

여러 파일이 있는 경우, 한 파일에서 다른 파일을 불러 오고 싶을 때는 import 라는 키워드를 이용한다.
현재 디렉토리에 두 파일이 모두 있는 경우  import "./파일이름.sol"; 과 같이 선언하여 파일을 불러온다.

Ex) 

```sol
import "./somethercontract.sol";

contract newContract is SomeOtherContract {

}
```



### 직접 해보기

<img width="670" alt="스크린샷 2020-01-28 오후 7 39 29" src="https://user-images.githubusercontent.com/54495632/73258059-3f749d00-4208-11ea-87f6-e0950584ef70.png">

새로운 파일 zombiefeeding.sol 에 zombiefactory.sol를 불러온다.

<img width="967" alt="스크린샷 2020-01-28 오후 7 57 08" src="https://user-images.githubusercontent.com/54495632/73258153-73e85900-4208-11ea-8e69-452bc62b547b.png">


다음과 같이 zombiefeeding.sol 파일과 zombiefactory.sol 파일이 존재한다.

zombiefeeding.sol 파일내용

```sol
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract ZombieFeeding is ZombieFactory {

}
```





## Chap 7. Storage Vs Memory


<img width="669" alt="스크린샷 2020-01-28 오후 7 59 21" src="https://user-images.githubusercontent.com/54495632/73258733-aba3d080-4209-11ea-9533-f9be9026b0ae.png">


<span style="color:red">Storage</span>는 블록체인 상에 영구적으로 저장되는 변수이다.
<span style="color:red">Memory</span>는 임시적으로 저장되는 변수로, 컨트랙트 함수에 대한 외부 호출 발생 사이에 지워진다. 

- storage : 하드 디스크
- memory : RAM 

과 비슷하다.

솔리디티 언어내에서 상태변수는 초기 설정으로 <span style="color:red">Storage</span>로 선언되어 블록체인에 영구적으로 저장되는 반면, 함수 내에 선언된 변수는 <span style="color:red">Memory</span>로 자동 선언되어서 함수 호출이 종료되면 사라진다.



### 구조체, 배열을 처리하는 경우

```sol
contract SandwichFactory{
struct Sandwich{
string name;
string status;
}

Sandwich[] sandwiches; // 구조체 배열 선언

function eatSandwich(uint _index) public {

Sandwich storage mySandwich=sandwiches[_index];
//솔리디티는 이 부분에서 mySandwich를 storage나 memory를 명시적으로 선언해야 한다는 경고 메세지를 출력한다.
//stroage 키워드로 선언
//mySandwich는 저장된 sandwiches[_index] 를 가리키는 포인터이다.

mySandwich.status="Eaten!";
//블록체인 상에서 sandwiches[_index] 을 영구적으로 변경한다.

Sandwich memory anotherSandwich=sandwiches[_index+1];
// 단순히 복사를 한다면 memory 키워드를 이용한다.
// anotherSandwich는 단순히 메모리에 데이터를 복사한다.

anotherSandwich.status="Eaten!";
// 임시변수인  anotherSandwich 를 변경한다.
// 원래 데이터인 sandwiches[_index +1 ] 에는 영향을 주지 않는다.

sandwiches[_index+1]=anotherSandwich;
//다음과 같은 경우는 임시 변경한 내용인 anotherSandwich를 블록체인 저장소에 저장하는 경우이다.
 }
}



### 직접 해보기

<img width="661" alt="스크린샷 2020-01-28 오후 7 59 42" src="https://user-images.githubusercontent.com/54495632/73260110-92505380-420c-11ea-9124-09bed4eda496.png">


​```sol
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract ZombieFeeding is ZombieFactory {

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
      require(msg.sender == zombieToOwner[_zombieId]);
      Zombie storage myZombie = zombies[_zombieId];
  } 

}
```





## Chap 8. 좀비 DNA




feedAndMultiply 함수를 마무리한다.
새로운 좀비의 DNA를 계산하는 공식을 함수로 작성하는데 먹이를 먹는 좀비의 DNA와 먹이가 되는 DNA의 평균을 낸다.

Ex)

```sol
function testDnaSplicing() public {
uint zombieDna=2222222222222222;
uint targetDna=4444444444444444;
uint newZombieDna=(zombieDna+targetDna) / 2 ;
// 결과로 333333333333333 가 나온다.
}
```





### 직접 해보기 

<img width="669" alt="스크린샷 2020-01-28 오후 8 43 13" src="https://user-images.githubusercontent.com/54495632/73261487-84e89880-420f-11ea-9661-45bc6a159480.png">


```sol
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract ZombieFeeding is ZombieFactory {

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna=_targetDna%dnaModulus;
    uint newDna=(myZombie.dna + _targetDna)/2;
    _createZombie("NoName",newDna);
  }

}
```





## Chap 9. 함수 접근 제어자

<img width="658" alt="스크린샷 2020-01-28 오후 9 16 47" src="https://user-images.githubusercontent.com/54495632/73263602-44d7e480-4214-11ea-8cfb-02732e4b161f.png">
<img width="649" alt="스크린샷 2020-01-28 오후 9 34 44" src="https://user-images.githubusercontent.com/54495632/73264416-10652800-4216-11ea-8b41-9f2e724560a2.png">


그 전 챕터에서 문제가 발생했다.
ZombieFactory내에 선언된 _createZombie 함수가 private 으로 선언됐는데
private함수는 해당 컨트랙트외에 다른 컨트랙트에서 접근할 수 없는 함수이다.
따라서, ZombieFeeding 컨트랙트에 상속시켜도 해당 컨트랙트 내에서는 _createZombie 함수를 사용할 수 없다.



### Internal 과 External

<img width="656" alt="스크린샷 2020-01-28 오후 9 34 50" src="https://user-images.githubusercontent.com/54495632/73264423-1529dc00-4216-11ea-8a9c-c9895c15634a.png">


- internal은 함수가 정의된 컨트랙트를 상속하는 컨트랙트에서도 접근이 가능하다. 
- external은 함수가 컨트랙트 바깥에서만 호출될 수 있고 컨트랙트 내의 다른 함수에 의해 호출될 수 없다.

internal, external 모두 함수 선언은 private이나 public 으로 함수를 선언하는 구문과 같다.


Ex)

```sol
contract Sandwich{
  uint private sandwichesEaten=0;
  
  function eat() internal {
  sandwichesEaten++;
 }
}

contract BLT is Sandwich {
 uint private baconSandwichesEaten = 0;

 function eatWithBacon() public returns (string) {
  baconSandwichesEaten++;
  
  eat();
  // Sandwich 컨트랙트에서 eat 함수가 internal 로 선언되서 
  // Sandwich 컨트랙트를 상속받은 BLT 컨트랙트에서 eat() 함수를 실행할 수 있다.
 }
}
```





### 직접 해보기

<img width="663" alt="스크린샷 2020-01-28 오후 9 16 51" src="https://user-images.githubusercontent.com/54495632/73264732-c6c90d00-4216-11ea-9e90-e98e92886e07.png">


zombiefactory.sol

```sol
pragma solidity ^0.4.19;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) internal {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender;
        ownerZombieCount[msg.sender]++;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        require(ownerZombieCount[msg.sender] == 0);
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}
```


zombiefeeding.sol

```sol
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract ZombieFeeding is ZombieFactory {

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }

}
```





## Chap 10. 좀비가 무엇을 먹나요?

<img width="672" alt="스크린샷 2020-01-28 오후 9 53 33" src="https://user-images.githubusercontent.com/54495632/73265880-258f8600-4219-11ea-863f-f758b1b699d2.png">

블록체인 플랫폼 안에서 다른 어플리케이션에 있는 데이터를 읽어와 현재 어플리케이션에 적용하는 것도 가능하다. 여기서는 크립토 키티의 스마트 컨트랙트가 public 으로 공개되어 있어 크립토 키티의 DNA 를 읽어와 크립토 좀비의 먹이로 주는 형식을 말하는 것 같다. 그러나 다른 어플리케이션의 스마트 컨트랙트가 공개 형식이라고 해서 해당 데이터를 write 할 권한은 없다(쓰고 삭제 불가능) Read에 대한 권한만 존재한다.



### 다른 컨트랙트와 상호작용하는 함수 / 인터페이스 선언

<img width="678" alt="스크린샷 2020-01-28 오후 9 56 41" src="https://user-images.githubusercontent.com/54495632/73266589-7fdd1680-421a-11ea-915f-601a039863fa.png">


블록체인 상에서 소유하지 않은 컨트랙트와 현재 컨트랙트가 상호작용 하기 위해선 
<span style="color:red">인터페이스</span>를 정의해야 한다.

Ex)

```sol
contract LuckyNumber{
 mapping(address=> uint) numbers;

 function setNum(uint _num) public {
 numbers[msg.sender]=_num;
}

function getNum(address _myAddress) public view returns (uint) {
 return numbers[_myAddress];
 }
}
```

이 컨트랙트는 자신의 LuckNum 을 저장할 수 있는 컨트랙트이다.
각자의 이더리움 주소에 따라 LuckyNum 값이 다르다. 즉, 이더리움 주소를 이용해 누구나 함수를 호출하면
자신의 LuckyNum을 찾을 수 있다.



<img width="679" alt="스크린샷 2020-01-28 오후 9 56 48" src="https://user-images.githubusercontent.com/54495632/73267202-e7e02c80-421b-11ea-805a-cd3ee87bc6ad.png">


```sol
contract NumberInterface {
function getNum(address _myAddress) public view returns (uint);
}
```

getNum :  이전 LuckyNum 컨트랙트에 있는 데이터를 읽고자 하는 external 함수

- 인터페이스 정의는 다른 컨트랙트와 상호작용하고자 하는 함수만을 선언한다. 다른 함수나 상태 변수를 언급하지 않는다.

- 함수 몸체를 정의하지 않는다. {} 중괄호를 쓰지 않고 함수 선언을 세미콜론(;) 으로 간단하게 끝낸다.

- 인터페이스는 컨트랙트 뼈대라고 볼 수 있다.

- app 코드에 인터페이스를 포함하면 컨트랙트는 다른 컨트랙트에 정의된 함수의 특성, 호출방법, 예상 응답을 알 수 있다.





### 직접 해보기

<img width="660" alt="스크린샷 2020-01-28 오후 10 21 54" src="https://user-images.githubusercontent.com/54495632/73267861-2c1ffc80-421d-11ea-9df0-03c051aead55.png">

getKitty function

```sol
function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
) {
    Kitty storage kit = kitties[_id];

    // if this variable is 0 then it's not gestating
    isGestating = (kit.siringWithId != 0);
    isReady = (kit.cooldownEndBlock <= block.number);
    cooldownIndex = uint256(kit.cooldownIndex);
    nextActionAt = uint256(kit.cooldownEndBlock);
    siringWithId = uint256(kit.siringWithId);
    birthTime = uint256(kit.birthTime);
    matronId = uint256(kit.matronId);
    sireId = uint256(kit.sireId);
    generation = uint256(kit.generation);
    genes = kit.genes;
}
```


<img width="672" alt="스크린샷 2020-01-28 오후 10 21 59" src="https://user-images.githubusercontent.com/54495632/73268538-748bea00-421e-11ea-8453-3e21b5914557.png">


js 와 다르게 솔리디티는 하나 이상의 값을 return 할 수 있다.

zombiefeeding.sol

```sol
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract KittyInterface {
    function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
    );
}

contract ZombieFeeding is ZombieFactory {

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }

}
```





## Chap 11. 인터페이스 활용하기

<img width="683" alt="스크린샷 2020-01-29 오후 4 56 36" src="https://user-images.githubusercontent.com/54495632/73337808-7dc99500-42b8-11ea-8cbb-66a6295f18b7.png">


인터페이스가 다음과 같이 정의된 상황에서 
인터페이스명 컨트랙트이름 = 인터페이스(해당인자 = 여기서는 컨트랙트 주소);
와 같이 다른 컨트랙트와 상호 작용할 수 있다.





### 직접 해보기


<img width="669" alt="스크린샷 2020-01-29 오후 4 56 42" src="https://user-images.githubusercontent.com/54495632/73338915-f5002880-42ba-11ea-9612-fda58967cbbe.png">


zombiefeeding.sol

```sol
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
  );
}

contract ZombieFeeding is ZombieFactory {

  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract=KittyInterface(ckAddress); // 이 부분

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }

}
```





## Chap 12. 다수의 변환값 처리하기


<img width="646" alt="스크린샷 2020-01-29 오후 5 18 28" src="https://user-images.githubusercontent.com/54495632/73339195-8bcce500-42bb-11ea-9d35-a3f08b583ce2.png">


다수의 반환값을 처리하기 위해서는
function 함수명() internal returns(타입1 변수이름1,타입2 변수이름2, 타입3 변수이름3, .....) {
return (1, 2, 3 ......);
}
다음과 같이 가능하다.

특정 또는 하나의 값에만 관심 있는 경우 다른 필드는 빈칸으로 놓으면 된다.

Ex)
함수 {
uint c;
( , ,c) = 함수명();
}

multipleReturns 함수에서는 uint 변수 3개 1,2,3 을 리턴한다.

processMultipleReturns() 함수에서는 
multipleReturns 함수에서 리턴되는 값 (1,2,3)을 uint형 변수 a,b,c 에 각각 할당한다.

getLastReturnValue() 함수에서는
특정 값 c 만 할당한다.





### 직접 해보기



<img width="665" alt="스크린샷 2020-01-29 오후 5 18 34" src="https://user-images.githubusercontent.com/54495632/73339545-57a5f400-42bc-11ea-8bc4-70e38a38e117.png">

getKitty 에서 return 되는 변수 중 genes이 마지막 변수 이므로 
9개의 , 콤마 후 kittyDna를 입력하여 할당한다.

```sol
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
  );
}

contract ZombieFeeding is ZombieFactory {

  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract = KittyInterface(ckAddress);

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }
//바로 밑 부터
  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna)=kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId,kittyDna);
  }
}
```





## Chap 13. 보너스 : 키티 유전자


<img width="661" alt="스크린샷 2020-01-29 오후 5 34 58" src="https://user-images.githubusercontent.com/54495632/73340230-b15aee00-42bd-11ea-9baf-f610429240df.png">


if문은 자바 스크립트와 동일하게 사용된다.





### 직접 해보기


<img width="666" alt="스크린샷 2020-01-29 오후 5 32 21" src="https://user-images.githubusercontent.com/54495632/73341242-97baa600-42bf-11ea-9d96-db18902b6d37.png">



```sol
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
  );
}

contract ZombieFeeding is ZombieFactory {

  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract = KittyInterface(ckAddress);

  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) public {//
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if(keccak256(_species) == keccak256("kitty")){// 해당 if문 수정
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna,"kitty");// 이부분 수정
  }
}
```
