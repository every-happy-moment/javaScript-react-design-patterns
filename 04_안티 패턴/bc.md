### 4. 안티패턴
- 겉으로만 해결책처럼 생긴 패턴.
- 프로젝트 라이프 사이클은 개발과 함께 시작.
	- 이 단계에서 좋은 디자인패턴을 선택한다.
	- 유지보수 단계에서 나쁜 패턴이 도입될 수 있어 조심해야한다.
- Javascript 안티패턴
	- 전역 컨텍스트에서 수많은 변수를 정의하여 전역 네임스페이스를 오염시키는 경우.
	- setTimeout 이나 setInverval에 함수가 아닌 문자열을 전달하여 동작에 오류를 범하는 경우.
	- Object 클래스의 프로토타입 수정
		- 최근 javscript에서는 불가능함.
	- document.createElement 대신에 document.write를 사용.
		- XHTML에서 사용되지 않는다.
		- createElement는 로드된 이후 페이지를 덮어씌워 보다 적합하다.
			- html 파서를 차단하고 렌더링을 중단시킬 수 있다.
			- 사용자 입력이나, 동적 데이터를 결합하면 XSS  공격에 취약할 수 있다.
			- 유연성과 유지보수성 측면에서 write 메서드는 문자열을 삽입하는 형태이기 때문에 코드가 복잡해질 수 있다.

```javascript
/** document.write 예제 */
document.write('<div class="example">Hello</div>');

/** document.createElement 예제 */
const div = document.createElement('div');
div.className = 'example'; div.textContent = 'Hello';
document.body.appendChild(div);
```
