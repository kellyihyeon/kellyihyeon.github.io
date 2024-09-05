---
title: Object - 1.객체, 설계
date: 2023-11-12 11:21:59
tags: 오브젝트, 객체, 설계
---
# 1. 티켓 판매 애플리케이션 구현하기

이벤트에 당첨되어 초대권을 가지고 있는 관람객과 그렇지 못한 관람객에게 티켓을 판매하는 애플리케이션을 구현해야합니다. 초대권을 가지고 있는 관람객은 초대장을 티켓으로 교환한 후 입장하고 초대권을 가지고 있지 않은 관람객은 티켓을 구매해서 입장을 해야 합니다.

## 코드 설명

이벤트 당첨자는 초대장을 가지고 있고, 이벤트에 당첨되지 않은 사람은 티켓을 구매할 현금을 가지고 있습니다. 관람객이 가지고 올 수 있는 물건은 현금, 초대장, 티켓 세가지가 있고 물건을 보관할 가방을 들고 올 수 있다고 가정합니다.

Bag 인스턴스의 상태는 두가지가 될 수 있습니다. 이벤트에 당첨되지 않은 관람객은 티켓을 구매해야 하니 현금만 보관할 수 있고, 이벤트 당첨자는 초대장과 현금을 보관할 수 있습니다.

```java
public class Invitation {

		private LocaldateTime when;
}
```

```java
public class Ticket {

		private Long fee;

		public Long getFee() { 
			return fee;
		}
	}
```

```java
public class Bag {

		private Long amount:

		private Invitation invitation; 

		private Ticket ticket;

		public Bag(long amount) {
				this(null, amount); 
		}

		public Bag(Invitation invitation, long amount) { 
			this.invitation = invitation;
			this.amount = amount;
		}

		public boolean hasInvitation() { 
			return invitation != null;
		}

		public boolean hasTicket() { 
			return ticket != null;
		}

		public void setTicket(Ticket ticket) { 
			this.ticket = ticket;
		}

		public void minusAmount (Long amount) { 
			this.amount -= amount;
		}

		public void plusAmount (Long amount) { 
			this.amount += amount;
		}
}
```

```java
public class Audience { 

		private Bag bag;

		public Audience(Bag bag) { 
			this.bag = bag;
		}

		public Bag getBag() { 
			return bag;
		}
}
```

관람객이 극장에 입장하려면 매표소를 거쳐야 합니다. 이벤트 당첨자는 매표소에서 초대장을 티켓으로 교환하고 그렇지 않은자는 티켓을 구매해야 합니다. 매표소는 관람객에게 판매할 티켓과 티켓의 판매금액을 보관하고 있습니다. 판매원은 매표소에서 초대장을 티켓으로 교환해 주거나 티켓을 판매하는 역할을 하고 있습니다.

```java
public class TicketOffice {

		private Long amount;

		private List<Ticket> tickets = new ArrayList<>();

		public TicketOffice(Long amount, Ticket . . . tickets) { 
				this.amount = amount;
				this.tickets.addAll(Arrays.aslist(tickets));
		}

		public Ticket getTicket() { 
				return tickets.remove(0);
		}

		public void minusAmount (Long amount) { 
				this.amount -= amount;
		}

		public void plusAmount(Long amount) { 
				this.amount += amount;
		}
		
}
```

```java
public class TicketSeller {

		private TicketOffice ticketOffice;

		public TicketSeller(TicketOffice ticketoffice) { 
				this.ticketoffice = ticketoffice;
		}

		public Ticketoffice getTicketOffice() { 
				return ticketoffice;
		}
}
```

해당 클래스들을 조합해서 극장이 관람객을 입장시키고 있는 로직입니다.

해당 코드를 살펴보면 극장은 관람객의 가방에 초대장이 있는지 확인을 합니다. 초대장이 들어 있으면 판매원에게서 티켓을 받아 관람객의 가방에 넣어줍니다. 가방에 초대장이 없는 경우에는 극장이 관람객의 가방에서 티켓 가격만큼 금액을 꺼내고나서 매표소에 티켓 가격만큼 금액을 증가시켜 놓습니다. 그리고 관람객의 가방에 티켓을 넣어줍니다.

```java
public class Theater {

		private TicketSeller ticketSeller;

		public Theater(TicketSeller ticketSeller) { 
				this.ticketSeller = ticketSeller;
		}

		public void enter (Audience audience) {
				if (audience.getBag().hasInvitation()) {
					Ticket ticket = ticketSeller.getTicketOffice().getTicket);
					audience.getBag().setTicket(ticket); 
				} else {
					Ticket ticket = ticketSeller.getTicketOffice().getTicket(); 
					audience.getBag().minusAmount(ticket.getFee()); 
					ticketSeller.getTicketOffice().plusAmount(ticket.getFee()); 
					audience.getBag().setTicket(ticket);
				}
		}
}
```

애플리케이션 구현이 끝났습니다. 작성된 프로그램을 실행하면 의도대로 동작하는 것을 확인할 수 있습니다. 하지만 해당 프로그램에는 몇가지 문제점이 있는데 어떤 문제점이 있는지 살펴봅시다.

# 2. 구현한 애플리케이션의 문제점

로버트 마틴이 소개한 소프트웨어 모듈이 가져야 하는 세 가지 기능을 적용하여 해당 어플리케이션이 어떤 문제점을 가지고 있는지 살펴보겠습니다.

```
소프트웨어 모듈이 가져야 하는 세 가지 목적
첫번째, 실행 중에 제대로 동작하는 것
두번째, 변경을 위해 존재하는 것
세번째, 코드를 읽는 사람과 의사소통 하는 것
```

Theater 클래스의 enter 메서드가 하는 일을 살펴봅시다.

관람객의 가방에 초대장이 있는지 확인합니다. 초대장이 있다면 판매원이 매표소에서 티켓을 꺼내 관람객의 가방에 넣어줍니다. 가방에 초대장이 없다면 판매원은 관람객의 가방에서 티켓 금액만큼 현금을 꺼내고 매표소에 현금을 넣어 놓습니다. 그리고 매표소에서 티켓을 꺼내 관람객의 가방에 넣어줍니다.

## 문제점1. 관람객과 판매원은 수동적인 존재이다

관람객의 관점에서 극장이 하는 일을 보면 자신의 가방을 마음대로 열어서 초대장을 확인하고 가방에 티켓을 넣어줍니다. 그리고 본인이 소유하고 있는 현금까지도 마음대로 빼가구요. 판매원의 관점에서도 마찬가지입니다. 극장은 자기가 일하는 매표소에서 티켓을 마음대로 빼가고 티켓 금액도 마음대로 넣어 놓습니다.

현실에서는 관람객이 가방에서 초대장을 꺼내 티켓과 교환합니다. 초대장이 없다면 현금을 내고 티켓을 구매합니다. 판매원은 자신이 일하는 매표소에서 관람객이 초대장을 주면 티켓으로 교환해주거나 티켓을 판매합니다.

코드에서 관람객과 판매원은 현실에서와 다르게 행동합니다. 코드 안에서 관람객과 판매원은 극장의 통제를 받는 수동적인 존재입니다. 그렇기 때문에 **코드를 읽는 사람과 제대로 된 의사소통을 할 수가 없습니다.**

## 문제점2. 극장은 모든 것을 기억해야 한다

```java
public void enter (Audience audience) {
				if (audience.getBag().hasInvitation()) {
					Ticket ticket = ticketSeller.getTicketOffice().getTicket);
					audience.getBag().setTicket(ticket); 
				} else {
					Ticket ticket = ticketSeller.getTicketOffice().getTicket(); 
					audience.getBag().minusAmount(ticket.getFee()); 
					ticketSeller.getTicketOffice().plusAmount(ticket.getFee()); 
					audience.getBag().setTicket(ticket);
				}
		}
```

극장의 enter 메서드가 하는 일을 다시 보면서 극장이 알고 있는 사항을 나열해 봅시다.

- 관람객이 가방을 가지고 있다
- 가방에는 초대장과 티켓과 현금이 있다
- 판매원의 매표소에 티켓과 현금이 있다

극장 클래스의 enter 메서드는 이 모든 세부사항을 알고 있습니다. 때문에 하나의 메서드 안에서 극장에서 발생하는 모든 일들을 처리하고 있습니다. 모든 세부사항을 써내려간 코드는 작성자 뿐만 아니라 **코드를 읽고 이해해야 하는 사람에게도 큰 부담이 됩니다.**

## 문제점3. 가정이 바뀌면 모든 코드가 흔들린다

관람객이 가방을 들고 있지 않거나 현금이 아닌 카드로 결제를 한다거나 판매원이 매표소가 아닌 곳에서 티켓을 판매한다면 코드에는 어떤 변화가 생길까요?

Audience 클래스에서 Bag을 제거합니다. 그리고 Theater 의 enter 메서드에서도 Bag에 접근하고 있는 코드를 전부 제거해야 합니다. 판매원의 매표소 또한 마찬가지 입니다. enter 메서드에서 매표소에 접근하고 있는 코드를 찾아서 제거해야 합니다.

Theater 클래스에서 Audience와 TicketSeller 의 세부적인 내용까지 전부 다 알고 있기 때문에, Audience와 TicketSeller에 변경이 일어나면 Theater 클래스까지 함께 변경해야 합니다. Theater 클래스가 두 클래스에 의존을 하고 있기 때문입니다. 객체 사이의 의존성이 높으면 결합도가 높다고 말합니다. 결국 의존성, 결합도는 변경과 관련이 있습니다. **객체 사이의 의존성, 결합도가 높으면 코드를 변경하기 어려워집니다.**

# 3. 설계 개선하기

예제 코드는 소프트웨어 모듈이 가져야하는 세가지 목적 중 한가지를 만족시킵니다. (실행 중에 제대로 동작하는 것) 하지만 변경하기가 어렵고, 코드를 읽는 사람이 이해하기가 어려워 의사소통 하는 데 문제가 있는 코드입니다. 두가지 목적도 만족시키기 위해서 코드를 개선해봅시다.

## 관람객과 판매원의 자율성을 높이자

Theater가 Audience와 TicketSeller에 관한 세부적인 내용을 알지 못하도록 만듭니다. Theater는 Audience가 Bag을 가지고 있으며 Bag에는 어떤 것들이 있는지 알아야 할 필요가 없습니다. TicketSeller에 관한 세부적인 내용도 알 필요가 없죠. Theater의 관심사는 관람객이 극장에 입장하는 것입니다.

관람객이 가방에서 초대장을 티켓으로 교환하거나 티켓을 구매하게 만들고, 판매원이 자신이 일하는 매표소에서 티켓을 판매하고 티켓 금액을 처리하게 만들면 코드가 개선될 것입니다.

Theater 의 enter 메서드에서 처리하고 있었던 해당 코드를 찾아서 각 객체 내부에 옮겨봅시다.

```java
public class Theater {

		private TicketSeller ticketSeller;

		public Theater(TicketSeller ticketSeller) { 
				this.ticketSeller = ticketSeller;
		}

		public void enter (Audience audience) {
//		if (audience.getBag().hasInvitation()) {
//			Ticket ticket = ticketSeller.getTicketOffice().getTicket);
//			audience.getBag().setTicket(ticket);
//		} else {
//			Ticket ticket = ticketSeller.getTicketOffice().getTicket();
//			audience.getBag().minusAmount(ticket.getFee());
//			ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
//			audience.getBag().setTicket(ticket);
//		}
		}
}
```

```java
public class TicketSeller {

		private TicketOffice ticketOffice;

		public TicketSeller(TicketOffice ticketoffice) { 
				this.ticketoffice = ticketoffice;
		}

//		public Ticketoffice getTicketOffice() { 
//				return ticketoffice;
//		}

		public void sellTo(Audience audience) {
				if (audience.getBag().hasInvitation()) {
					Ticket ticket = ticketOffice.getTicket);
					audience.getBag().setTicket(ticket); 
				} else {
					Ticket ticket = ticketOffice.getTicket);
					audience.getBag().minusAmount(ticket.getFee()); 
					ticketOffice.plusAmount(ticket.getFee()); 
					audience.getBag().setTicket(ticket);
				}
		}
}
```

enter 메서드에서 직접 TicketOffice에 접근하고 있던 코드를 전부 TicketSeller의 sellTo 메서드로 옮겼습니다. 이제 외부에서 TicketOffice에 직접 접근할 수 있는 방법이 없기 때문에 getTicketOffice 메서드는 제거가 되었습니다. TicketSeller 가 TicketOffice에서 티켓을 꺼내고 판매하고 티켓 금액을 처리할 수 있게 되었어요.

```java
**💡 이어지는 개념**
캡슐화: 개념적이나 물리적으로 객체 내부의 세부적인 사항을 감추는 것
캡슐화의 목적: 객체 사이의 결합도를 낮추어 변경하기 쉬운 객체를 만드는 것
```

TicketSeller의 sellTo 메서드에서는 여전히 Audience 가 수동적인 존재입니다. 이번엔 TicketSeller가 Audience의 Bag에 직접 접근을 하고 있어요. 이번엔 Bag에 접근하는 코드를 전부 옮겨봅시다.

```java
public class Audience { 

		private Bag bag;

		public Audience(Bag bag) { 
			this.bag = bag;
		}

//		public Bag getBag() { 
//			return bag;
//		}

		public Long buy(Ticket ticket) { 
				if (bag.hasInvitation()) {
					bag.setTicket(ticket); 
					return 0L;
				} else {
					bag.setTicket(ticket); 
					bag.minusAmount(ticket.getFee()); 
					return ticket.getFee();
				}
		}
}
```

이제는 Audience가 초대장이 있는지 스스로 확인할 수 있게 되었습니다. Audience가 Bag을 직접 처리하고 있기 때문에 외부에서는 Bag에 대한 접근을 할 수가 없도록 getBag 메서드를 제거하였습니다.

내부 구현을 캡슐화하였더니 객체들 사이의 결합도가 낮아졌습니다. 가정이 바뀌어도 Audience와 TicketSeller 의 코드 변경이 Theater에 영향을 미치지 않습니다.

```java
public class Theater {

		private TicketSeller ticketSeller;

		public Theater(TicketSeller ticketSeller) { 
				this.ticketSeller = ticketSeller;
		}

		public void enter (Audience audience) {
				ticketSeller.sellTo(audience);
		}
}
```

```java
public class TicketSeller {

		private TicketOffice ticketOffice;

		public TicketSeller(TicketOffice ticketoffice) { 
				this.ticketoffice = ticketoffice;
		

		public void sellTo(Audience audience) {
				ticketoffice.plusAmount(audience.buy(ticketoffice.getTicket()));
		}
}
```

```java
public class Audience { 

		private Bag bag;

		public Audience(Bag bag) { 
			this.bag = bag;
		}

		public Long buy(Ticket ticket) { 
				if (bag.hasInvitation()) {
					bag.setTicket(ticket); 
					return 0L;
				} else {
					bag.setTicket(ticket); 
					bag.minusAmount(ticket.getFee()); 
					return ticket.getFee();
				}
		}
}
```

소프트웨어 모듈이 가져야 하는 세가지 목적을 다시 한번 살펴봅시다.

- 첫번째, 실행 중에 제대로 동작하는 것
- 두번째, 변경을 위해 존재하는 것
- 세번째, 코드를 읽는 사람과 의사소통 하는 것

기존 코드에서는 두번째, 세번째의 목적을 만족시키지 못했지만 수정된 코드는 이를 만족시킵니다.

## 캡슐화와 응집도

Theater의 enter 메서드에서 Audience의 Bag 클래스, TicketSeller의 TicketOffice 클래스에 직접 접근 하고 있던 부분을 차단하고 Audience와 TicketSeller에서 직접 처리할 수 있게 하였습니다. Theater 클래스는 이제 더이상 Audience와 TicketSeller의 내부 구현에 대해서는 알지 못합니다. 그리고 객체들은 서로 메시지를 주고 받으며 의사소통 할 수 있게 되었습니다.

밀접하게 연관되어 있는 작업만 수행하고 연관성이 없는 작업은 다른 객체에게 위임하는 객체를 보고 응집도가 높다고 말합니다. 이는 즉 **객체의 응집도를 높이려면 객체 스스로가 자신의 데이터를 책임져야 한다**는 것을 의미합니다.

## 책임의 이동

코드를 변경 하기 전의 절차적 설계에서는 Theater에 모든 책임이 있었습니다. enter 메서드에서 관람객이 극장에 입장하는 데 필요한 모든 절차를 처리하고 있었어요. 변경 후의 객체지향 설계에서는 각각의 객체가 자신이 맡은 일을 처리했습니다. Theater에 몰려 있던 책임이 개별 객체로 이동한 것입니다.

설계를 어렵게 만드는 것은 **의존성**입니다. 이 문제를 해결하기 위한 방법은 객체들 사이의 불필요한 의존성을 찾아내고 이를 제거해서 객체 사이의 결합도를 느슨하게 낮추어 놓는 것입니다. 예제 코드에서는 결합도를 낮추기 위해서 Audience와 TicketSeller 내부에 세부사항들을 **캡슐화** 하였습니다.