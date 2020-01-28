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