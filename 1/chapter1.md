# CHAPTER 1: ABOUT THIS MANUAL

# 1.1 INTEL® 64 AND IA-32 PROCESSORS COVERED IN THIS MANUAL
지원되는 cpu 목록 나열됨.

# 1.2 OVERVIEW OF VOLUME 1: BASIC ARCHITECTURE
volume 1에 대한 설명

# 1.3 NOTATIONAL CONVENTIONS
이 문서에서 사용하는 표기법에 대한 설명

## 1.3.1 Bit and Byte Order
메모리주소는 위로, 왼쪽으로 갈수록 커짐, 리틀 엔디안 사용

## 1.3.2 Reserved Bits and Software Compatibility
예약 비트와 소프트웨어 호환성
레지스터와 메모리 레이아웃 설명에서, **reserved** 라고 표현된 비트가 있다. 예약 비트들은 나중에 호환성을 위해 사용하는 것이다.   
예약 비트 처리할때 다음 가이드 라인을 따라야 한다:
- reserved bit가 포함된 레지스터의 값을 테스트 할때 그 값에 의존하지 말것, 테스트 하기 전에 해당 비트들을 마스크 할 것.
- 메모리나 레지서터에 저장할 땐 reserved bit의 상태에 의존하지 말것
- reserved bit에 남남겨진 정보에 의존하지 말 것
- 레지스터에 로딩할 때, 항상 문서에 적혀있는 값으로 reserved bits를 채울 것, 없다면 기존 값을 사용할 것

### 1.3.2.1 Instruction Operands
명령어들이 상징벅으로 표현될 때, IA-32의 어셈블리 언어의 서브셋이 사용된다. 이 서브셋에서는 다음과 같은 포맷을 따른다:
    label: mnemonic argument1, argument2, argument3
- label은 :을 뒤에 붙이는 식별자
- mnemonic은 명령어 opcode에 대해 지정된 이름
- argument1~3은 옵션임. opcode에 따라 0~3개의 인수가 존재함. 존재할때는 리터럴 값이나 데이터 항목의 식별자 형태임. 피연산자 식별자(Operand identifiers)는 레지스터의 예약된 이름이나 다른 프로그램에서 선언된 데이터 항목들에게 할당된 것으로 추청되는 것들이다.

산술 연산자나 논리 연산자에 두개의 피연산자가 있을땐, 오른쪽이 source 왼쪽이 destination.
   
예
    LOADREG: MOV EAX, SUBTOTAL
LOADREG은 label, MOV는 opcode에 대한 연상 기호 식별자, EAX는 destination 피연산자, SUBTOTAL은 source 피연산자. 일부 어셈블리 언어는 source와  destination을 반대 순서로 씀.(at&t)

## 1.3.3 Hexadecimal and Binary Numbers
16진수는 뒤에 H를 붙여 표현->0F82EH   
2진수는 뒤에 B를 붙여 표현->1010B

## 1.3.4 Segmented Addressing
프로세서는 바이트 주소를 사용함. 이것은 메모리가 바이트 수열로 구성되고, 접근할 수 있다는걸 의미함. 한개 이상의 바이트를 접근할 때 바이트 주소는 바이트 메모리를 찾기 위해 사용됨. 주소를 매길 수 있는 메모리를 주소 체계(address space)라고 부름

프로세서는 세그먼트 어드레싱를 지원함. 세그먼트 어드레싱은 세그먼트라고 불리는 많고 독립된 메모리 주소 공간을 가진 주소 형태다. 예를들어 프로그램은 해당 프로그램의 코드와 스택을 각각의 세그먼트에 유지할 수 있다. 코드 주소들은 항상 코드 공간을 가르켜야한다, 그리고 스택 주소는 항상 스택 공간을 가르켜야한다. 다음 표현은 세그먼트 내의 바이트 주소를 지정할 때 사용된다:
    Segment-register:Byte-address
예를 들어, 다음 세그먼트 주소 식별자는 DS세그먼트의 FF79H주소를 가르킨다:
    DS:ff79H
다음 세그먼트 주소 식별자는 코드 세그먼트(CS)의 명령어 주소를 가르킨다. CS레지스터는 코드 세그먼트(CS)를 가르키고 EIP레지스터는 명령어의 주소를 담고 있다.
    CS:EIP

## 1.3.5 A New Syntax for CPUID, CR, and MSR Values
CIPID 명령어를 사용하여 특징 플레그, 상태, 그리고 시스템 정보를 얻을 수 있다, control register비트를 확인하고 모델 특정 레지스터를 읽음으로써.

## 1.3.6 Exceptions
예외는 전형적으로 명령어가 오류를 발생시킬때 발생하는 이벤트다. 예를들어 0으로 나누기는 예외를 발생시킨다. 하지만 중단점과 같은 일부예외는, 다른 상태에서 발생한다. 일부 형식의 예외는 에러 코드를 제공한다. 에러 코드는 에러에 대해 추가 정보를 보고한다. 다음은 예외와 에러코드의 표기법의 예다:
    #PF(fault code)
위 예제는 fault의 종류의 에러코드가 보고된 상태의 page-fault예외를 가르킨다.(fault code가 있는 page-fault라는 뜻) 몇몇 상황에서는 예외는 정확한 에러 코드를 만들어 내지 않을 수 있다. 아래와 같은 경우에는 general-protection예외에 에러코드는 0이다.
    #GP(0)
# 1.4 RELATED LITERATURE
관련 자료 목록