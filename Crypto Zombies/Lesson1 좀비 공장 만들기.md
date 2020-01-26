# Crypto Zombies

이 문서는 https://cryptozombies.io 를 토대로 작성했습니다.





# Lesson1 좀비 공장 만들기





# Chap2. 컨트랙트



<img width="557" alt="스크린샷 2020-01-25 오후 10 37 46" src="https://user-images.githubusercontent.com/54495632/73122052-7017c500-3fc3-11ea-9246-36d36dcd9858.png">

솔리디티 코드는 컨트랙트 안에 싸여 있다. 컨트랙트란 이더리움 디앱의 기본 구성 요소로,
모든 변수와 함수는 어느 한 컨트랙트에 속한다.

- Ex) empty 상태의 HelloWorld 컨트랙트는 다음과 같이 표현할 수 있다.
  contract HelloWorld {

}







# Version Pragma

<img width="651" alt="스크린샷 2020-01-25 오후 10 40 51" src="https://user-images.githubusercontent.com/54495632/73122133-9be77a80-3fc4-11ea-8a6d-2657426cdc64.png">


모든 솔리디티 코드는  version pragma로 시작해야된다. 코드가 이용할 버전을 선택하는 부분.
조금 귀찮은 언어인것 같다.


- Ex) 0.4.19 버전은 다음과 같이 선언할 수 있다. pragma solidity ^0.4.19;





# 직접 구현해보기

<img width="378" alt="스크린샷 2020-01-25 오후 10 50 07" src="https://user-images.githubusercontent.com/54495632/73122159-10221e00-3fc5-11ea-9869-2adf15742df0.png">

pragma solidity ^0.4.19;

contract ZombieFactory{

}



<img width="529" alt="스크린샷 2020-01-25 오후 10 43 01" src="https://user-images.githubusercontent.com/54495632/73122197-7444e200-3fc5-11ea-8fdb-d8370d24bb2d.png">







# Chap 3. 상태 변수 & 정수

<img width="397" alt="스크린샷 2020-01-25 오후 10 58 31" src="https://user-images.githubusercontent.com/54495632/73122269-457b3b80-3fc6-11ea-937d-bf1c94b1cc4b.png">

상태 변수는 컨트랙트 저장소에 영구적으로 저장된다. 이더리움 블록체인에 기록되는데 데이터 베이스에 데이터를 쓰는 것과 동일하다고 한다. 
블록체인이 정보를 기록하는 데이터베이스와 같은 느낌+ 영구적 이라는 특징을 동시에 갖고 있는 것 같다.







- 부호 없는 정수 uint

<img width="400" alt="스크린샷 2020-01-25 오후 10 58 37" src="https://user-images.githubusercontent.com/54495632/73122272-4744ff00-3fc6-11ea-809c-88ac8c281609.png">

Ex) contract Example {
// 이 변수는 블록체인에 영구적으로 저장된다
uint myUnsignedInteger = 100;
}

uint 자료형은 부호 없는 정수를 의미한다.(unsigned int 인 듯 싶다.)
uint 자료형은 실제로 uint256을 의미하고 뒤에 숫자는 비트 사이즈를 의미한다.
uint16 uint32 와 같이 더 작은 비트로 선언할 수도 있다.





- 직접 해보기

<img width="396" alt="스크린샷 2020-01-25 오후 10 58 43" src="https://user-images.githubusercontent.com/54495632/73122273-4a3fef80-3fc6-11ea-81e8-b50458ab842a.png">

pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;

}







# Chap 4. 수학 연산


<img width="396" alt="스크린샷 2020-01-25 오후 11 04 54" src="https://user-images.githubusercontent.com/54495632/73122351-2204c080-3fc7-11ea-9f38-1f4ae8201b78.png">


기존 프로그래밍 언어들과 연산은 비슷하다. 
지수 연산을 제공하는데 ** 와 같이 할 수 있다.

- 덧셈 : x + y
- 뺄셈 : x - y
- 곱셈 : x * y
- 나눗셈 : x / y
- 나머지 : x % y
- 지수 : x ** y (x^y를 의미)





<img width="406" alt="스크린샷 2020-01-25 오후 11 08 02" src="https://user-images.githubusercontent.com/54495632/73122375-90498300-3fc7-11ea-886c-98a0f6212bfd.png">

pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

}





# Chap 5. 구조체





<img width="676" alt="스크린샷 2020-01-25 오후 11 12 02" src="https://user-images.githubusercontent.com/54495632/73122420-2382b880-3fc8-11ea-8715-4d52b795acd9.png">

다른 프로그래밍 언어와 마찬가지로 구조체 기능을 제공한다.
Person 이라는 구조체 안에 uint 변수형 age, string 변수형 name이 존재한다.
사용할 해당 컨트랙트 안에 선언해야된다.(다른 점)




- String

<img width="670" alt="스크린샷 2020-01-25 오후 11 12 06" src="https://user-images.githubusercontent.com/54495632/73122423-254c7c00-3fc8-11ea-8b27-5d45d07d4931.png">

솔리디티에서 string 변수 타입도 제공한다. 임의의 길이를 가졌고 UTF-8 데이터이다.





- 직접 해보기

<img width="680" alt="스크린샷 2020-01-25 오후 11 12 10" src="https://user-images.githubusercontent.com/54495632/73122424-27aed600-3fc8-11ea-88f1-368965cdf037.png">


pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    
    struct Zombie {
      string name;
      uint dna;
    }

}







# Chap 6. 배열






<img width="658" alt="스크린샷 2020-01-25 오후 11 17 02" src="https://user-images.githubusercontent.com/54495632/73122481-d6531680-3fc8-11ea-9177-ce354f5000da.png">



솔리디티 언어에서는 동적 배열, 정적 배열을 모두 제공하며

정적 배열은 고정적인 길이를 가지는 배열을 의미하며 다음과 같이 선언할 수 있다.
타입[크기] 배열이름;

Ex)
uint[2] fixedArray;
string[5] stringArray;



동적 배열은 고정된 크기가 없고 계속 크기가 커질 수 있는 배열을 의미하며 다음과 같이 선언할 수 있다.
타입[] 배열이름;

Ex)
uint[] dynamicArray;




구조체의 배열을 생성할 수도 있다. 
struct Person{

}
다음과 같은 Person 구조체를 이용하면

Person[] people; // 동적 구조체 배열 , 원소를 추가할 수 있다.





- Public 배열 


<img width="660" alt="스크린샷 2020-01-25 오후 11 17 06" src="https://user-images.githubusercontent.com/54495632/73122487-deab5180-3fc8-11ea-809c-a253a0f9af7e.png">



솔리디티에서 배열을 public으로 선언할 수 있는데 기존 다른 프로그래밍 언어에서 이용했던 public 선언과 의미가 동일하다. (컨트랙트에 공개 데이터를 저장할 때 이용한다.)
배열에 대한 read 권한은 갖게 되지만 외부에서 write는 불가능하다.

public 배열 선언을 위해 getter 메소드가 자동적으로 실행되며
Person[] public people; 과 같이 배열 명에 public을 선언하면 된다.



- 직접 해보기

  

<img width="656" alt="스크린샷 2020-01-25 오후 11 17 10" src="https://user-images.githubusercontent.com/54495632/73122488-e2d76f00-3fc8-11ea-8221-0bc106fda8cf.png">

Zombie 구조체를 public 배열로 선언하고 배열 이름은 zombies

pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    
    struct Zombie {
        string name;
        uint dna;
    }
    
    Zombie[] public zombies;

}







# Chap 7. 함수 선언



<img width="685" alt="스크린샷 2020-01-25 오후 11 31 08" src="https://user-images.githubusercontent.com/54495632/73122680-eec43080-3fca-11ea-804d-81244fc66014.png">


솔리디티에서 함수 기능이 존재하고 
함수 선언은 function 키워드를 이용한다.
function 함수이름(파라미터){

}

기존 언어들과 다른 부분이 없다.

Ex)

function eatHamburgers(string _name, uint _amount) {

}

eatHamburgers 라는 함수로 string 타입의 _name 변수 , uint 타입의 _amout 변수를 파라미터로 받고 있다.  

솔리디티언어에서는 함수 파라미터명을 _ 언더 스코어로 시작하여 전역 변수와 구별한다고 한다.

함수의 호출은
함수명(해당 파라미터); 로 가능하다.
Ex) eatHamburgers("vitalik",100);







- 직접 해보기



<img width="685" alt="스크린샷 2020-01-25 오후 11 31 08" src="https://user-images.githubusercontent.com/54495632/73122680-eec43080-3fca-11ea-804d-81244fc66014.png">



createZombie 라는 함수를 생성, _name(string), _dna(uint) 두 개의 파라미터가 존재

pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    
    struct Zombie {
        string name;
        uint dna;
    }
    
    Zombie[] public zombies;
    
    function createZombie(string _name, uint _dna){
        
    }

}






# Chap 8. 구조체와 배열 활용하기





<img width="662" alt="스크린샷 2020-01-26 오후 8 24 12" src="https://user-images.githubusercontent.com/54495632/73134436-df4ff080-4079-11ea-8b37-f8d519afe341.png">

- 구조체 배열 생성
구조체이름[] (public) 배열이름;
Person[] public people;
-> people이라는 이름의 구조체 배열을 생성했다. Person 구조체를 참조함

- 새로운 Person을 구조체로부터 생성하고 people 배열에 추가하는 방법
push 를 이용한다.

배열이름.push(생성했던 Person구조체 이름);

Ex)
Person satoshi = Person(172,"Satoshi"); //satoshi
people.push(satoshi);
-> people.push(Person(172,"Satoshi"); // 와 같이 한 줄로 표현할 수 있다.




- push() 는 배열의 끝에 추가된다. vector에서 push_back과 같다.

Ex)
uint[] numbers;
numbers.push(5);
numbers.push(10);
numbers.push(15);
// numbers 배열은 [5, 10, 15]과 같다.







- 직접 해보기 

  


<img width="671" alt="스크린샷 2020-01-26 오후 8 24 20" src="https://user-images.githubusercontent.com/54495632/73134440-e5de6800-4079-11ea-8dda-5246694de456.png">


함수에 코드를 넣어 새로운 Zombie를 생성하여 zombies 배열에 추가하도록 한다. 
새로운 좀비를 위한 name과 dna는 createZombie함수의 인자값이어야 한다.
코드를 한 줄로 간결하게 작성해 보자.


pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    
    struct Zombie {
        string name;
        uint dna;
    }
    
    Zombie[] public zombies;
    
    function createZombie(string _name, uint _dna) {
        zombies.push(Zombie(_name,_dna));
    }

}





# Chap 9. Private / Public 함수




<img width="662" alt="스크린샷 2020-01-26 오후 9 03 44" src="https://user-images.githubusercontent.com/54495632/73134904-6a7fb500-407f-11ea-802c-f25b4cc636b7.png">


솔리디티 언어에서 함수의 선언은 기본적으로 public 으로 선언된다.
즉, 외부에서 컨트랙트의 함수를 호출하여 실행이 가능하다.

private을 이용하면 함수를 외부에 공개하지 않을 수 있다.
공개할 함수만 public으로 선언하는 것이 취약점을 제거하여 보안성을 높일 수 있는 방법이다.

- private 선언

function _함수이름(파라미터) private{

}과 같이 선언한다.
private 함수는 파라미터와 마찬가지로 _ 언더바로 시작하게 적는다.


Ex ) 
uint[] numbers;

function _addToArray(uint _number) private{
 numbers.push(_number);
}

컨트랙트 내의 다른 함수들만이 _addToArray 함수를 호출하여 numbers 배열에 추가할 수 있다.





- 직접 해보기


<img width="658" alt="스크린샷 2020-01-26 오후 9 03 47" src="https://user-images.githubusercontent.com/54495632/73134959-204b0380-4080-11ea-84f5-05af1bc80ca9.png">

public 으로 선언된 함수를 private 선언으로 변경하면 된다.

pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    
    struct Zombie {
        string name;
        uint dna;
    }
    
    Zombie[] public zombies;
    
    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    }

}





# Chap 10. 함수 더 알아보기

return 값과 함수 제어자에 대해 다룬다.



<img width="562" alt="스크린샷 2020-01-26 오후 10 28 03" src="https://user-images.githubusercontent.com/54495632/73135899-28f50700-408b-11ea-92da-e6b960b332b7.png">



- 반환값

함수에서 값을 반환 받는 경우 return과 반환할 값을 선언해준다.
returns (타입) 으로 함수 자체의 타입도 선언해줘야 한다.

Ex) 
string  greeting = "What's up dog";

function sayHello() public returns (string) {
  return greeting;
}





- 함수 제어자 




<img width="678" alt="스크린샷 2020-01-26 오후 9 11 10" src="https://user-images.githubusercontent.com/54495632/73135009-e0d0e700-4080-11ea-9b70-506352003f53.png">




- returns (타입) 으로만 함수를 선언한 경우 return값의 상태를 변화시키지 않는다.
리턴 값을 변경시키고 싶은 경우 함수를 view 로 선언한다.

​       Ex )

​       function sayHello() public view returns (string){}




- pure함수는 앱의 어떤 데이터에도 접근하지 않는 함수이다.

​       파라미터에 대한 리턴 값만 변경한다.

​       Ex )

​        function _multiply (uint a, uint b) private pure returns(uint) { 
​        return a*b;
​        }





- 직접 해보기

<img width="681" alt="스크린샷 2020-01-26 오후 9 11 19" src="https://user-images.githubusercontent.com/54495632/73135114-175b3180-4082-11ea-8012-b84d105ba341.png">



pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    
    struct Zombie {
        string name;
        uint dna;
    }
    
    Zombie[] public zombies;
    
    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    }
    
    function _generateRandomDna(string _str) private view returns (uint){ 
    
    }

}







# Chap 11. Keccak256과 형 변환





<img width="656" alt="스크린샷 2020-01-26 오후 9 24 47" src="https://user-images.githubusercontent.com/54495632/73135140-55f0ec00-4082-11ea-95a1-524954600ead.png">





앞서 만든 함수에서 uint 타입형인 랜덤 값을 return 하기 위해서는 
SHA3로 해싱하는   Keccak256 솔리디티 내장 해싱 함수를 이용한다.

 Keccak256 는 입력되는 string을 256비트 , 16진수로 맵핑한다.

Ex ) 
keccak256("aaaab");  6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5 와 같이 해싱값이 나온다.







- 형변환

<img width="671" alt="스크린샷 2020-01-26 오후 9 24 52" src="https://user-images.githubusercontent.com/54495632/73135229-1e367400-4083-11ea-8208-e1fd5f9bc99a.png">


연산시 변수의 자료형이 다른 경우 에러가 발생한다.
이 때 형변환이 필요한데 타입명(해당변수); 를 통해 형변환이 가능하다.

Ex)

uint8 a=5;
uint b=6;

uint8 c =a*b; // a*b 가 uint8이 아닌 uint 를 반환하여 에러 발생

uint8 c = a * uint8(b); // b를 uint8으로 형변환하여 전체 a*b 연산이 uint8 을 반환



- 직 접 해보기

<img width="673" alt="스크린샷 2020-01-26 오후 9 24 56" src="https://user-images.githubusercontent.com/54495632/73135212-f21af300-4082-11ea-8021-893dd74d47d2.png">


1. keccak256(_str) 로 해싱된 값을 uint 로 형 변환한 후
uint 형 타입의 rand 변수에 할당한다.
2. return rand%dnaModulus; 

pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    
    struct Zombie {
        string name;
        uint dna;
    }
    
    Zombie[] public zombies;
    
    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    } 
    
    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand%dnaModulus;
    }

}





# Chap 12. 종합하기




<img width="676" alt="스크린샷 2020-01-26 오후 9 36 12" src="https://user-images.githubusercontent.com/54495632/73135312-eaa81980-4083-11ea-8da8-7f88aa705092.png">


pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    
    struct Zombie {
        string name;
        uint dna;
    }
    
    Zombie[] public zombies;
    
    function _createRandomZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    } 
    
    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }
    
    function createRandomZombie(string _name) public { //1 public 함수 생성 
        uint randDna = _generateRandomDna(_name); //2 rnadDna 변수에 결과 값 저장
        _createZombie(_name,randDna);// 파라미터값을 주고 해당 함수 호출 
    }

}

private으로 선언한 것과 마찬가지로 함수를 public 으로 생성할 것
- 이라는 조건이 있어 function에 public을 따로 붙여줘야된다.





# Chap 13. 이벤트




<img width="660" alt="스크린샷 2020-01-26 오후 9 43 32" src="https://user-images.githubusercontent.com/54495632/73135416-f34d1f80-4084-11ea-9790-fa0c38a884cb.png">



이벤트는 해당되는 액션이 발생했음을 어플리케이션에게 알리는 것이다.

Ex)

event IntegersAdded(uint x, uint y, uint result);
//이벤트 선언

function add(uint _x, uint _y) public {
 uint result = _x+_y; 
IntegersAdded(_x, _y, result); //이벤트를 실행하여 앱에게 add 함수가 실행됐음을 알린다.
return result;
}


만약 js와 연동한다면 앱에서 해당 이벤트에 대한 처리는 다음과 같이 가능하다.

YourContract.IntegersAdded(function(error, result)){
//action
}





- 직접 해보기


<img width="664" alt="스크린샷 2020-01-26 오후 9 58 30" src="https://user-images.githubusercontent.com/54495632/73135572-052fc200-4087-11ea-8d2e-abf84664e29b.png">


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
    
    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) -1;
        NewZombie(id,_name,_dna);
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



# Chap 14. Web3.js



<img width="662" alt="스크린샷 2020-01-26 오후 10 13 41" src="https://user-images.githubusercontent.com/54495632/73135737-209bcc80-4089-11ea-9804-e7553acf316c.png">



chap13. 까지 모든 컨트랙트가 완성됐고
프론트엔드로 넘어와 web3 js 로 완성하는데 
이번 레슨에서는 주제가 아닌 것 같다.




// 여기에 우리가 만든 컨트랙트에 접근하는 방법을 제시한다:
var abi = /* abi generated by the compiler */
var ZombieFactoryContract = web3.eth.contract(abi)
var contractAddress = /* our contract address on Ethereum after deploying */
var ZombieFactory = ZombieFactoryContract.at(contractAddress)
// `ZombieFactory`는 우리 컨트랙트의 public 함수와 이벤트에 접근할 수 있다.

// 일종의 이벤트 리스너가 텍스트 입력값을 취한다:
$("#ourButton").click(function(e) {
  var name = $("#nameInput").val()
  // 우리 컨트랙트의 `createRandomZombie`함수를 호출한다:
  ZombieFactory.createRandomZombie(name)
})

// `NewZombie` 이벤트가 발생하면 사용자 인터페이스를 업데이트한다
var event = ZombieFactory.NewZombie(function(error, result) {
  if (error) return
  generateZombie(result.zombieId, result.name, result.dna)
})

// 좀비 DNA 값을 받아서 이미지를 업데이트한다
function generateZombie(id, name, dna) {
  let dnaStr = String(dna)
  // DNA 값이 16자리 수보다 작은 경우 앞 자리를 0으로 채운다
  while (dnaStr.length < 16)
    dnaStr = "0" + dnaStr

  let zombieDetails = {
    // 첫 2자리는 머리의 타입을 결정한다. 머리 타입에는 7가지가 있다. 그래서 모듈로(%) 7 연산을 하여
    // 0에서 6 중 하나의 값을 얻고 여기에 1을 더해서 1에서 7까지의 숫자를 만든다. 
    // 이를 기초로 "head1.png"에서 "head7.png" 중 하나의 이미지를 불러온다:
    headChoice: dnaStr.substring(0, 2) % 7 + 1,
    // 두번째 2자리는 눈 모양을 결정한다. 눈 모양에는 11가지가 있다:
    eyeChoice: dnaStr.substring(2, 4) % 11 + 1,
    // 셔츠 타입에는 6가지가 있다:
    shirtChoice: dnaStr.substring(4, 6) % 6 + 1,
    // 마지막 6자리는 색깔을 결정하며, 360도(degree)까지 지원하는 CSS의 "filter: hue-rotate"를 이용하여 아래와 같이 업데이트된다:
    skinColorChoice: parseInt(dnaStr.substring(6, 8) / 100 * 360),
    eyeColorChoice: parseInt(dnaStr.substring(8, 10) / 100 * 360),
    clothesColorChoice: parseInt(dnaStr.substring(10, 12) / 100 * 360),
    zombieName: name,
    zombieDescription: "A Level 1 CryptoZombie",
  }
  return zombieDetails
}



다음과 같은 js 코드를 통해 웹 상에서 이름을 입력했을 때 크립토 좀비가 랜덤으로 생성이 된다.



<img width="1344" alt="스크린샷 2020-01-26 오후 10 18 20" src="https://user-images.githubusercontent.com/54495632/73135794-c94a2c00-4089-11ea-880a-8819e0c59c01.png">