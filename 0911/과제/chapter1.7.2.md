## 1.7.2 Algorithm Workbench

### 1. Write a function that receives a string containing a 16-bit binary integer
* **번역:** 16비트 2진수 정수가 포함된 문자열을 받아, 그 문자열의 정수 값을 반환하는 함수를 작성하세요.
* **답:**
    문자열을 왼쪽부터 순회하며 2진법의 자릿값 원리에 따라 누적 합계를 계산하는 알고리즘을 사용합니다.
* **설명:**
    결과를 담을 변수를 0으로 시작합니다. 그 후 문자열의 각 자리를 순회하며 기존 결과에 2를 곱하고 현재 자리의 숫자를 더하는 과정을 반복합니다.
    ```javascript
    function binaryToInt(binStr) {
      let number = 0;
      for (const digit of binStr) {
        number = number * 2 + parseInt(digit);
      }
      return number;
    }
    ```

***
### 2. Write a function that receives a string containing a 32-bit hexadecimal integer
* **번역:** 32비트 16진수 정수가 포함된 문자열을 받아, 그 문자열의 정수 값을 반환하는 함수를 작성하세요.
* **답:**
    1번과 동일한 누적 합계 알고리즘을 사용하되, 밑을 16으로 변경하여 적용합니다.
* **설명:**
    결과 변수를 0으로 초기화하고, 문자열을 왼쪽부터 순회합니다. 반복마다 현재 결과에 16을 곱하고, 다음 자리의 16진수 값('A'~'F'는 10~15로 변환)을 더해줍니다.
    ```javascript
    function hexToInt(hexStr) {
      let number = 0;
      const hexMap = "0123456789ABCDEF";
      for (const char of hexStr.toUpperCase()) {
        const digitValue = hexMap.indexOf(char);
        number = number * 16 + digitValue;
      }
      return number;
    }
    ```

***
### 3. Write a function that receives an integer
* **번역:** 정수를 하나 받아, 그 숫자의 2진수 표현이 담긴 문자열을 반환하는 함수를 작성하세요.
* **답:**
    입력받은 정수를 몫이 0이 될 때까지 2로 반복해서 나누고, 각 단계의 나머지를 취하는 알고리즘을 사용합니다.
* **설명:**
    예를 들어 13을 2진수로 바꾸려면, 13을 2로 나눈 나머지 `1`을 기록하고, 몫인 6을 다시 2로 나눕니다. 이 과정을 몫이 0이 될 때까지 반복한 뒤, 기록된 나머지들을 역순으로 조합하면 "1101"이라는 2진수 문자열을 얻을 수 있습니다.
    ```javascript
    function intToBinary(number) {
      if (number === 0) return "0";
      let binaryStr = "";
      while (number > 0) {
        binaryStr = (number % 2) + binaryStr;
        number = Math.floor(number / 2);
      }
      return binaryStr;
    }
    ```

***
### 4. Write a function that receives an integer
* **번역:** 정수를 하나 받아, 그 숫자의 16진수 표현이 담긴 문자열을 반환하는 함수를 작성하세요.
* **답:**
    3번과 동일하게 반복적인 나눗셈 알고리즘을 사용하되, 2 대신 16으로 나눕니다.
* **설명:**
    정수를 16으로 계속 나누면서 나머지를 구합니다. 나머지가 10 이상일 경우, 해당하는 16진수 문자('A'~'F')로 변환합니다. 구해진 나머지들을 역순으로 조합하여 최종 16진수 문자열을 만듭니다.
    ```javascript
    function intToHex(number) {
      if (number === 0) return "0";
      const hexMap = "0123456789ABCDEF";
      let hexStr = "";
      while (number > 0) {
        hexStr = hexMap[number % 16] + hexStr;
        number = Math.floor(number / 16);
      }
      return hexStr;
    }
    ```

***
### 5. Write a function that adds two digit strings in base b
* **번역:** 2에서 10 사이의 밑(b)을 사용하는 두 개의 숫자 문자열을 더하는 함수를 작성하세요.
* **답:**
    초등학교에서 배운 세로셈 덧셈과 동일한 원리를 알고리즘으로 구현합니다.
* **설명:**
    두 문자열의 가장 오른쪽 자리부터 덧셈을 시작합니다. 각 자리의 숫자와 이전 자리에서 발생한 올림수(carry)를 더하고, 그 합을 밑(b)으로 나눕니다. 나머지는 결과 자리에 쓰고, 몫은 다음 자리 계산을 위한 올림수로 사용합니다.
    ```javascript
    function addBaseB(s1, s2, b) {
      let i = s1.length - 1;
      let j = s2.length - 1;
      let carry = 0;
      let result = "";
      
      while (i >= 0 || j >= 0 || carry > 0) {
        const d1 = i >= 0 ? parseInt(s1[i]) : 0;
        const d2 = j >= 0 ? parseInt(s2[j]) : 0;
        
        const sumVal = d1 + d2 + carry;
        result = (sumVal % b).toString() + result;
        carry = Math.floor(sumVal / b);
        
        i--; j--;
      }
      return result;
    }
    ```

***
### 6. Write a function that adds two hexadecimal strings
* **번역:** 각각 최대 1,000자리인 두 개의 16진수 문자열을 더하는 함수를 작성하세요.
* **답:**
    5번 문제의 세로셈 덧셈 알고리즘을 밑이 16인 경우에 맞춰 적용합니다.
* **설명:**
    덧셈 과정에서 16진수 문자 'A'~'F'를 숫자 10~15로 변환하여 계산합니다. 각 자리의 합이 16을 넘어가면 올림수가 발생하며, 결과 또한 10~15에 해당하는 값은 다시 'A'~'F' 문자로 변환하여 저장합니다.
    ```javascript
    function addHex(s1, s2) {
      const hexMap = "0123456789ABCDEF";
      let i = s1.length - 1;
      let j = s2.length - 1;
      let carry = 0;
      let result = "";

      while (i >= 0 || j >= 0 || carry > 0) {
        const d1 = i >= 0 ? hexMap.indexOf(s1[i].toUpperCase()) : 0;
        const d2 = j >= 0 ? hexMap.indexOf(s2[j].toUpperCase()) : 0;
        
        const sumVal = d1 + d2 + carry;
        result = hexMap[sumVal % 16] + result;
        carry = Math.floor(sumVal / 16);
        
        i--; j--;
      }
      return result;
    }
    ```

***
### 7. Write a function that multiplies a single hexadecimal digit by a hexadecimal digit string
* **번역:** 한 자리 16진수와 최대 1,000자리인 16진수 문자열을 곱하는 함수를 작성하세요.
* **답:**
    세로셈 곱셈과 동일한 원리를 알고리즘으로 구현합니다.
* **설명:**
    긴 문자열의 가장 오른쪽 자리부터 한 자리 숫자와 곱셈을 시작하여 올림수를 관리하며 왼쪽으로 진행합니다. 각 단계의 계산은 16진법에 맞춰 처리합니다.
    ```javascript
    function multiplyHex(longStr, digitChar) {
      const hexMap = "0123456789ABCDEF";
      const d2 = hexMap.indexOf(digitChar.toUpperCase());
      let i = longStr.length - 1;
      let carry = 0;
      let result = "";

      while (i >= 0 || carry > 0) {
        const d1 = i >= 0 ? hexMap.indexOf(longStr[i].toUpperCase()) : 0;
        
        const prodVal = (d1 * d2) + carry;
        result = hexMap[prodVal % 16] + result;
        carry = Math.floor(prodVal / 16);
        
        i--;
      }
      return result;
    }
    ```

***
### 8. Write a Java program that contains the calculation shown below
* **번역:** `int X = (Y + 4) * 3;` 코드를 디스어셈블하고 각 줄의 목적에 대한 주석을 추가하세요.
* **답:**
    고급 언어 코드를 컴파일러가 번역한 저수준 명령어(바이트코드)로 변환하여 단계별 실행 과정을 확인합니다.
* **설명:**
    `X = (Y + 4) * 3;`이라는 한 줄의 코드는 컴퓨터 내부에서 다음과 같이 여러 개의 단순한 명령어로 나뉘어 순차적으로 실행됩니다.
    
    **Java Code (`Calculation.java`):**
    ```java
    public class Calculation {
        public static void main(String[] args) {
            int Y = 10;
            int X = (Y + 4) * 3;
        }
    }
    ```
    **결과 (바이트코드 및 주석):**
    ```
      Code:
         0: bipush        10     // 1. 정수 10을 스택에 push
         2: istore_1             // 2. 스택의 값(10)을 1번 지역 변수(Y)에 저장
         3: iload_1              // 3. 1번 지역 변수(Y)의 값을 스택에 load
         4: iconst_4             // 4. 정수 4를 스택에 push
         5: iadd                 // 5. 스택의 두 값(Y와 4)을 더하고 결과를 스택에 push
         6: iconst_3             // 6. 정수 3을 스택에 push
         7: imul                 // 7. 스택의 두 값((Y+4)와 3)을 곱하고 결과를 스택에 push
         8: istore_2             // 8. 최종 결과를 2번 지역 변수(X)에 저장
         9: return               // 9. 메소드 종료
    ```

***
### 9. Devise a way of subtracting unsigned binary integers
* **번역:** 부호 없는 2진수를 빼는 방법을 고안하고 시험해 보세요.
* **답:**
    10진수 뺄셈과 유사하게, 윗자리에서 값을 빌려오는(borrow) 방식을 사용합니다.
* **설명:**
    10진수에서는 윗자리에서 빌려오면 `10`이 더해지지만, 2진수에서는 `2`가 더해집니다. `0`에서 `1`을 빼야 할 경우, 왼쪽으로 이동하며 `1`인 자리를 찾아 값을 빌려와야 합니다. 이 과정에서 중간에 있던 `0`들은 `1`이 되고, 최종적으로 `0`이었던 자리는 `2`가 되어 뺄셈을 수행합니다.
    
    **테스트 1: `10001000` - `00000101` = `10000011`**
    
    **테스트 2: `1101` - `0110` = `0111`**
    
    **테스트 3: `10000` - `0001` = `01111`**
