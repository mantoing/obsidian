---
날짜: '"2024-07-03"'
넘버: 
태그: 42과제정리
출처: 
aliases:
---
### 날짜  2024-07-03 19:51

### 태그: #42과제 

>[!메모]
>

### 원문
---

# ft_printf

### 개요

| 함수 이름           | ft_printf                                    |
| --------------- | -------------------------------------------- |
| **프로토타입**       | `int ft_printf(const char *, ...);`          |
| **제출할 파일**      | `ft_printf.c`                                |
| **매개변수**        | `const char *ag` 포맷 문자열 `...` 가변인자 목록        |
| **반환값**         | 성공적으로 출력된 문자 수 <br> `ERR` - 입력 문자열이 NULL일 경우 |
| **사용가능한 외부 함수** | `va_start`, `va_arg`, `va_end`               |

- printf 함수를 직접 구현해보자.
- **ft_printf 함수**는 주어진 포맷 문자열과 가변 인자를 받아 처리하는 함수이다.
- 함수는 포맷 문자열을 `ag`로 받아, 내부적으로 가변 인자를 처리하여 출력 형식을 지정한다.
- `va_start`와 `va_end` 매크로를 사용하여 가변 인자 목록을 초기화하고 정리한다.
- `par_arg`라는 내부 함수를 호출하여 실제로 인자를 처리한다.
- 입력 문자열이 NULL일 경우 `ERR`를 반환한다.

### 가변인자(Variadic Arguments)?

가변 인자는 함수에 전달할 인자의 개수가 고정되지 않고, 호출할 때마다 다를 수 있는 기능을 의미한다. 즉, 함수가 몇 개의 인자를 받을지 미리 정해져 있지 않은 경우에 사용된다. 가변 인자를 사용하면 같은 함수로 다양한 개수의 인자를 처리할 수 있어 매우 유용한다.

#### C 언어에서의 가변 인자

C 언어에서는 가변 인자를 사용하는 함수를 만들 때 `stdarg.h` 헤더 파일을 포함하고, `va_list`, `va_start`, `va_arg`, `va_end` 등의 매크로를 사용합니다. 이를 통해 가변 인자를 처리할 수 있습니다.

### stdarg.h

```c
#include <stdarg.h>

void va_start(va_list ap, last);
type va_arg(va_list ap, type);
void va_end(va_list ap);
void va_copy(va_list dest, va_list src);
```

- 헤더에서 va_list를 선언하고 세 가지 매크로를 정의하는데, 이를 이용해 피 호출 함수에게 개수와 타입이 알려져 있지 않은 인자들의 목록을 순회할 수 있다. 피호출 함수에서 va_list타입 객체를 선언해야 하며 그 객체를 매크로 `va_start()`, `va_arg()`, `va_end()` 에서 사용한다.

- **va_start()**
	`va_start()` 매크로는 이후 `va_arg()`, `va_end()`가 쓸 수 있게 ap를 초기화 하며, 따라서 가장 먼저 호출해야한다. 
	인자 last는 가변 인자 목록 전의 마지막 인자, 즉 호출 함수에서 타입을 알고 있는 마지막 인자의 이름이다. 
	이 인자의 주소를 `va_start()`매크로 내에서 사용할 수도 있으므로 레지스터 변수로 선언되어 있거나 함수나 배열 타입으로 선언되어 있어선 안 된다.

- **va_arg()**
	`va_arg()` 매크로는 호출의 다음 인자의 타입과 값을 가진 식으로 확장된다. 
	인자 ap는 `va_start()`로 초기화 한 va_list ap이다. 
	`va_arg ()` 를 호출할 때마다 ap를 변경해서 다음 호출 때 다음 인자를 반환하도록 한다. 
	인자 type은 타입 이름으로, type에 * 만 붙이면 지정한 타입의 객체에 대한 포인터 타입을 얻을 수 있도록 지정한다. 
	`va_start()` 매크로 다음으로 처음 `va_arg()` 매크로를 쓰면 last 다음 인자를 반환한다. 이어지는 호출이 나머지 인자들의 값을 반환한다. 
	다음 인자가 없거나 type이 (기본 인자 승격 방식에 따라 승격된) 실제 다음 인자의 타입과 호환되지 않는 경우 정해져 있지 않은 오류가 발생하게 된다. ap를 어떤 함수로 전달하고 그 함수에서 `va_arg(ap, type)`을 사용하는 경우에 함수 반환 후 ap의 값은 규정되어 있지 않다.

- **va_end()**
	각 `va_start()` 호출에는 같은 함수 내에 대응하는 `va_end()` 호출이 있어야 한다. `va_end(ap)` 호출 후 변수 ap는 규정되어 있지 않다. `va_start()` 와 `va_end()`로 각각 감싸서 목록을 여러 번 순회 하는 것이 가능하다. `va_end()`가 매크로일 수도 있고 함수일 수도 있다.

- **va_copy()**
	`va_copy()` 매크로는 (앞서 초기화 한) 가변 인자 목록 src를 dest로 복사한다. dest에 같은 last인자로 `va_start()`를 적용하고 이어서 현재 src상태에 도달하기까지 한 것과 같은 횟수의 `va_arg()`호출을 적용한 것처럼 동작한다. 단순 명백한 구현 방식에서는 va_list가 가변 인자 함수의 스택 프레임에 대한 포인터일 것이다. 그런 (가장 흔한)방식에서는 다음 할당이 아무 문제가 없을 것이다.
	```c
	va_list aq = ap;
	```
	하지만 안타깝게도 va_list를 포인터의 (길이 1인)배열로 만드는 시스템도 있으며, 거기선 다음과 같이 해야한다.
	```c
va_list aq;
*aq = *ap;
	```

	그리고 레지스터로 인자를 전달하는 시스템에서는 `va_start()`에서 메모리를 할당해서 인자들을 저장하고 `va_arg()`가 목록을 순회할 수 있도록 다음 인자가 뭔지 표시도 해 두어야 할 수도 있다. 그럼 그 할당 메모리를 `va_end()`에서 다시 해제할 수 있다. 이런 상황에 대응하기 위해 C99에서는 매크로 `va_copy()`를 추가하여 위 할당을 다음 코드로 대체 할 수 있도록 한다.
	```c
	va_list aq; 
	va_copy(aq, ap); 
	... va_end(aq);
	```
	각 ``va_copy()`` 호출에는 같은 함수 내에 대응하는 ``va_end()``호출이 있어야 한다. 일부 시스템에서는 ``va_copy()``를 제공하지 않고 대신  ``__va_copy``가 있는데, 제안 초안에서 사용했던 이름이다.
	```c
	void printNumbers(int args, ...)    // 가변 인자의 개수를 받음, ...로 가변 인자 설정
{
    va_list ap;    // 가변 인자 목록 포인터

    va_start(ap, args);    // 가변 인자 목록 포인터 설정
    for (int i = 0; i < args; i++)    // 가변 인자 개수만큼 반복
    {
        int num = va_arg(ap, int);    // int 크기만큼 가변 인자 목록 포인터에서 값을 가져옴
                                      // ap를 int 크기만큼 순방향으로 이동
        printf("%d ", num);           // 가변 인자 값 출력
    }
    va_end(ap);    // 가변 인자 목록 포인터를 NULL로 초기화

    printf("\n");    // 줄바꿈
}
	```


### 구현

- **ft_printf**

	```c
		int	ft_printf(const char *ag, ...)
	{
		int		npf;
		va_list	ap;
		char	*s;
	
		if (!ag)
			return (ERR);
		va_start (ap, ag);
		s = (char *)ag;
		npf = par_arg(ap, s);
		va_end(ap);
		return (npf);
	}
	```

	stdarg헤더 의 va_를 사용하여 원하는 형식의 바이트까지 밀어줄수있게 메인구성.
	par_arg함수로 npf로 어떤 형식인지. 어떤 주소값설정을 어찌할지 넘겨준다.
	반환값은 처리한 npf함수로 반환.

- **par_arg.c**
	```c
	int	par_arg(va_list ap, char *ag)
{
	int	npf;

	npf = 0;
	while (*ag)
	{
		if (*ag != '%')
		{
			write(1, ag, 1);
			++npf;
		}
		else
		{
			++ag;
			if (print_format_type(ap, *ag, &npf) == ERR)
				return (ERR);
		}
		++ag;
	}
	return (npf);
}
	```
	%를 만나지 않을때까지 ag를 write로 출력하고 npf의 주소를 넘겨준다. 
	만약 %를 만났을때는 형식을 걸러주기위해 print_format_type함수로 넘겨주게 되는데 만약 여기서 ERR를 만난다면 에러표시.

- **print_format_type**
	```c
	int	print_format_type(va_list ap, char type, int *npf)
{
	int	res;

	if (type == 'd' || type == 'i')
		res = prtnbr(type, va_arg(ap, int));
	else if (type == 'u' || type == 'x' || type == 'X')
		res = prtnbr(type, va_arg(ap, unsigned int));
	else if (type == 'p')
		res = prtnbr(type, va_arg(ap, unsigned long));
	else if (type == 's')
		res = prtstr(va_arg(ap, char *));
	else if (type == 'c')
		res = prtchar((char)va_arg(ap, int));
	else if (type == '%')
		res = prtchar('%');
	else
		return (ERR);
	*npf += res;
	return (res);
}
	```
	%d, %i, %u, %x, %X, %p 일 경우에는 숫자 출력과 계산을 해주는 prtnbr함수로 넘겨주어 마무리 처리를 하게 된다. 
	%s일 경우에는 문자열을 출력해주어야 하기 때문에 문자열을 출력할수 있게 해주는 prtstr함수로 넘겨서 마무리 할수 있게 한다.
	%c일 경우에는 문자를 출력해주어야 하기 때문에 문자를 출력할수 있게 해주는 prtchar함수로 넘겨서 마무리 할수 있게 한다. 
	%만 들어왔을때는 그대로 %출력 npf에 주소값을 계속 더해줘서 뒤로 넘겨준다.

- **prtchar**
	```c
	int	prtchar(char c)
{
	int	res;

	res = 0;
	if (ft_putchar((char)c) == ERR)
		return (ERR);
	return (1);
}
	//else
		//res = ft_putchar(c);
	```

	t_putchar함수에 넣어서 출력해주면서 에러를 만났을때는 -1반환,
	처음에는 else 조건문도 넣어 주어서 res에 출력을 해주었지만. 위에 if문에서 한번 출력을하고 또 출력이 되는 오류가 발생하여서 삭제해 주었다.

- **prtstr**
	```c
	int	prtstr(char *str)
{
	int	s_len;

	s_len = ft_strlen(str);
	if (!str)
		s_len = (int)write(1, "(null)", 6);
	else
		s_len = ft_putstr(str);
	return (s_len);
}
	```
	길이를 세주는 함수를 사용.
	그리고 문자열에 오류가 있을경우 따로 NULL출력.

- **prtnbr**
	```c
	#include "ft_printf.h"

char	*chk_base(char type)
{
	if (type == 'd' || type == 'i' || type == 'u')
		return ("0123456789");
	else if (type == 'x' || type == 'p')
		return ("0123456789abcdef");
	return ("0123456789ABCDEF");
}

int	put_nbr(char type, unsigned long n, char *base)
{
	int				res;
	int				temp;
	unsigned long	nbr;

	temp = 0;
	if (type == 'd' || type == 'i' || type == 'u')
	{
		nbr = 10;
		if (n >= nbr)
			temp = put_nbr(type, n / nbr, base);
	}
	else
	{
		nbr = 16;
		if (n >= nbr)
			temp = put_nbr(type, n / nbr, base);
	}
	if (temp == ERR)
		return (ERR);
	res = temp;
	temp = write(1, &base[n % nbr], 1);
	if (temp == ERR)
		return (ERR);
	res = res + temp;
	return (res);
}

int	prtnbr(char type, unsigned long n)
{
	int		tmp;
	int		res;
	char	*base;

	base = chk_base(type);
	res = 0;
	if (type == 'p')
	{
		res = write(1, "0x", 2);
	}
	else if ((type == 'd' || type == 'i') && (int)n < 0)
	{
		res = write(1, "-", 1);
		n *= -1;
	}
	if (res == ERR)
		return (ERR);
	tmp = put_nbr(type, n, base);
	if (tmp == ERR)
		return (ERR);
	res = res + tmp;
	return (res);
}
	```
	number관련해서 처리하기 위한 로직을 추가하였다.



### 구현후 느낀점

코드들이 항상 정해진 인자만 받는다고 생각을 했는데 가변인자라는 개념을 알게되어서 신기했고 재밌었다. printf를 그냥 사용하기만 했지 이렇게 어려운 과정으로 기능을 하는지 알수있었다. 전체를 구현하지않고 몇가지의 경우만 구현을 해서 살짝 아쉽다. 다음 리팩토링때는 더 많은 타입을 출력할수있게 만들고싶다.


---
### 생각(파생된 질문/생각)

### 출처

### 연결 문서 (연결 이유)