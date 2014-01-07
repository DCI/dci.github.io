---
layout: tabbed
title: C++ example
tabs:
  - Top
  - Data Classes
  - Context
  - Roles
  - Utilities
  - Main
---

## <a name="Data Classes">[Data Classes](#Data Classes)

```cpp
/*
 *  Account.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */


#ifndef _ACCOUNT_H
#define _ACCOUNT_H

#include <string>
using namespace std;

class Account {
public:
	string accountID(void) const;
	Account(void);
private:
	int acct;
};

#endif
```

```cpp
/*
 *  CheckingAccount.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/17/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */


#ifndef _CHECKINGACCOUNT_H
#define _CHECKINGACCOUNT_H

#include "Account.h"
#include "Currency.h"
#include "TransferMoneySource.h"
#include "MyTime.h"
#include <string>

using namespace std;

class XferMoneyContext;
class PayBillsContext;

class CheckingAccount:
		public Account,
		public TransferMoneySink {
public:
	CheckingAccount(void);
private:
	// These functions can be virtual if there are
	// specializations of CheckingAccount
	Currency availableBalance(void);
	void decreaseBalance(Currency);
	void increaseBalance(Currency);
	void updateLog(string, MyTime, Currency);
private:
	Currency availableBalance_;
};

#endif
```

```cpp
/*
 *  InvestmentAccount.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _INVESTMENTACCOUNT_H
#define _INVESTMENTACCOUNT_H

#include "Account.h"
#include "Currency.h"
#include "TransferMoneySource.h"
#include "MyTime.h"
#include <string>

using namespace std;

class XferMoneyContext;
class PayBillsContext;

class InvestmentAccount:
	public Account,
	public TransferMoneySource {
public:
friend class TransferMoneySource;
	InvestmentAccount(void);
	Currency availableBalance(void);
	void increaseBalance(Currency);
	void decreaseBalance(Currency);
	void updateLog(string, MyTime, Currency);
private:
	Currency availableBalance_;
};
 /*
 *  InvestmentAccount.cpp
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#include "InvestmentAccount.h"
#include "PayBillsContext.h"
#include "TransferMoneyContext.h"
#include <string>
#include <iostream>
using namespace std;

InvestmentAccount::InvestmentAccount(void):
	availableBalance_(Euro(0.00))
{
}

Currency InvestmentAccount::availableBalance(void) {
	std::cout << "InvestmentAccount::availableBalance returns "
		<< availableBalance_ << std::endl;
	return availableBalance_;
}


void
InvestmentAccount::increaseBalance(Currency c) {
	std::cout << "InvestmentAccount::increaseBalance(" << c << ")" << std::endl;
	availableBalance_ += c;
}

void
InvestmentAccount::decreaseBalance(Currency c) {
	std::cout << "InvestmentAccount::decreaseBalance(" << c << ")" << std::endl;
	availableBalance_ -= c;
}


void
InvestmentAccount::updateLog(string s, MyTime, Currency c) {
	std::cout << "account: " << accountID() << " InvestmentAccount::updateLog(\"" <<  s << "\", Time, " << c << ")" << std::endl;
}

#endif
```

```cpp
/*
 *  SavingsAccount.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _SAVINGSACCOUNT_H
#define _SAVINGSACCOUNT_H

#include "Account.h"
#include "TransferMoneySource.h"
#include "MyTime.h"
#include <string>

using namespace std;

class XferMoneyContext;
class PayBillsContext;

class SavingsAccount:
		public Account,
		public TransferMoneySink {
friend class TransferMoneySink;	// optional
public:
	SavingsAccount(void);
private:
	// These functions can be virtual if there are
	// specializations of SavingsAccount
	Currency availableBalance(void);
	void decreaseBalance(Currency);
	void increaseBalance(Currency);
	void updateLog(string, MyTime, Currency);
private:
	Currency availableBalance_;
};
 /*
 *  SavngsAccount.cpp
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#include "SavingsAccount.h"
#include "PayBillsContext.h"
#include "TransferMoneyContext.h"
#include <iostream>

SavingsAccount::SavingsAccount(void):
	availableBalance_(Euro(0.00))
{
	assert(this != NULL);
	
	// no application code
	
	assert(availableBalance_ == Euro(0.0));
	assert(true);
}

Currency SavingsAccount::availableBalance(void) {
	assert(this != NULL);
	std::cout << "SavingsAccount::availableBalance returns "
		<< availableBalance_ << std::endl;
	assert(true);
	return availableBalance_;
}

void SavingsAccount::decreaseBalance(Currency c) {
	assert(this != NULL);
	std::cout << "SavingsAccount::decreaseBalance(" << c << ")" << std::endl;
	assert(c > availableBalance_);
	availableBalance_ -= c;
	assert(availableBalance_ > Euro(0.0));
}

const unsigned MAX_BUFFER_SIZE = 256;

void SavingsAccount::updateLog(string logMessage,
				MyTime timeOfTransaction,
				Currency amountForTransaction) {
	assert(this != NULL);
	assert(logMessage.size() > 0);
	assert(logMessage.size() < MAX_BUFFER_SIZE);
	assert(timeOfTransaction > MyTime("00:00:00.00 1970/1/1"));
	std::cout << "account: " << accountID() << " SavingsAccount::updateLog(\"" << logMessage << "\", MyTime, " << amountForTransaction << ")"
		<< std::endl;
	assert(true);
}

void SavingsAccount::increaseBalance(Currency c) {
	assert(this != NULL);
	std::cout << "SavingsAccount::increaseBalance(" << c << ")" << std::endl;
	availableBalance_ += c;
	assert(true);
}

#endif
```

```cpp
/*
 *  Creditor.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/17/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _CREDITOR_H
#define _CREDITOR_H

#include "Currency.h"

class MoneySink;

class Creditor
{
public:
	virtual MoneySink *account(void) const = 0;
	virtual Currency amountOwed(void) const = 0;
};

class ElectricCompany: public Creditor
{
public:
	ElectricCompany(void);
	MoneySink *account(void) const;
	Currency amountOwed(void) const;
private:
	MoneySink *account_;
};

class GasCompany: public Creditor
{
public:
	GasCompany(void);
	MoneySink *account(void) const;
	Currency amountOwed(void) const;
private:
	MoneySink *account_;
};

#endif
```

```cpp
/*
 *  Creditor.cpp
 *  AgileBook
 *
 *  Created by James Coplien on 9/17/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#include "Creditor.h"
#include "SavingsAccount.h"
#include "CheckingAccount.h"

ElectricCompany::ElectricCompany(void)
{
	account_ = new CheckingAccount;
}

MoneySink*
ElectricCompany::account(void) const
{
	return account_;
}

Currency
ElectricCompany::amountOwed(void) const
{
	return Euro(15.0);
}

GasCompany::GasCompany(void)
{
	account_ = new SavingsAccount;
	account_->increaseBalance(Euro(500.00));	// start off with a balance of 500
}

MoneySink*
GasCompany::account(void) const
{
	return account_;
}
	
Currency
GasCompany::amountOwed(void) const
{
	return Euro(18.76);
}
```

## <a name="Utilities">[Utilities](#Utilities)

```cpp
/*
 *  Currency.h
 *  AgileBook
 *
 *  Created by James Coplien on 7/31/09.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */


#ifndef _CURRENCY_H
#define _CURRENCY_H

#include <string>
#include <sstream>

class Currency;

class CurrencyBase {
friend class Currency;
public:
	virtual CurrencyBase &operator+=(const Currency& amount) = 0;
	virtual CurrencyBase &operator-=(const Currency& amount) = 0;
	virtual CurrencyBase &operator=(const Currency& amount) = 0;
	CurrencyBase(void): referenceCount_(1) { }
	virtual ~CurrencyBase() {  }
	virtual std::string name(void) const = 0;
	virtual std::string sign(void) const = 0;
	virtual double amountInEuro(void) const = 0;
	virtual std::string asString(void) const = 0;
protected:
	unsigned referenceCount_;
};


class Currency {
public:
	virtual Currency &operator+=(const Currency& amount) {
		*actualCurrency_ += amount;
		return *this;
	}
	virtual Currency &operator-=(const Currency& amount) {
		*actualCurrency_ -= amount;
		return *this;
	}
	virtual Currency &operator=(const Currency& amount) {
		amount.actualCurrency_->referenceCount_++;
		if (--actualCurrency_->referenceCount_ <= 0) delete actualCurrency_;
		actualCurrency_ = amount.actualCurrency_;
		return *this;
	}
	Currency(const Currency& amount) {
		(actualCurrency_ = amount.actualCurrency_)->referenceCount_++;
	}
	Currency(void);
	virtual ~Currency() { if (--actualCurrency_->referenceCount_ <= 0) delete actualCurrency_; }
	virtual std::string name(void) const { return actualCurrency_->name(); }
	virtual std::string sign(void) const { return actualCurrency_->sign(); }
	virtual double amountInEuro(void) const { return actualCurrency_->amountInEuro(); }
	virtual std::string asString(void) const { return actualCurrency_->asString(); }
friend std::ostream &operator<<(std::ostream &stream, Currency ¤cy)
	{
		stream << currency.asString();
		return stream;
	}
	bool operator<(const Currency &other) { return amountInEuro() < other.amountInEuro(); }
	bool operator==(const Currency &other) { return amountInEuro() == other.amountInEuro(); }
	bool operator>(const Currency &other) { return amountInEuro() > other.amountInEuro(); }
	bool operator<=(const Currency &other) { return amountInEuro() <= other.amountInEuro(); }
	bool operator>=(const Currency &other) { return amountInEuro() >= other.amountInEuro(); }
	Currency(CurrencyBase *derivedClassThis): actualCurrency_(derivedClassThis) { }
private:
	CurrencyBase *actualCurrency_;
};


class Euro: public CurrencyBase {
friend class Currency;
public:
	operator Currency(void) {
		return Currency(this->copy());
	}
	CurrencyBase *copy(void) const {
		return new Euro(amount_);
	}
	virtual Euro &operator+=(const Currency& amount) {
		amount_ += amount.amountInEuro();
		return *this;
	}
	virtual Euro &operator-=(const Currency& amount) {
		amount_ -= amount.amountInEuro();
		return *this;
	}
	virtual Euro &operator=(const Currency& amount) {
		amount_ = amount.amountInEuro();
		return *this;
	}
	Euro(double amount = 0.0): amount_(amount) { }
	virtual std::string name(void) const { return "Euro"; }
	virtual std::string sign(void) const { return "€"; }
	virtual double amountInEuro(void) const { return amount_; }
	virtual std::string asString(void) const {
		std::ostringstream stream;
		stream << amount_;
		return stream.str();
	}
private:
	double amount_;
};

inline Currency::Currency(void): actualCurrency_(new Euro) {  }

#endif
```

```cpp
/*
 *  Globals.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _GLOBALS_H
#define _GLOBALS_H

#include "MyTime.h"

extern void endTransaction(void);
extern void beginTransaction(void);
extern MyTime DateTime(void);

#endif
 /*
 *  GlobalsImplementation.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */
 
 
#include "MyTime.h"
#include <iostream>

void endTransaction(void) {
	std::cout << "::endTransaction(void)" << std::endl;
}

void beginTransaction(void) {
	std::cout << "::beginTransaction(void)" << std::endl;
}

MyTime DateTime(void) {
	return 0;
}
```

```cpp
/*
 *  MyExceptions.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _MYEXCEPTIONS_H
#define _MYEXCEPTIONS_H

class InsufficientFunds {
public:
	InsufficientFunds(void);
};


#endif
```

```cpp
/*
 *  MyTime.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _MYTIME_H
#define _MYTIME_H
#include <string>

class MyTime {
public:
	MyTime(void);
	MyTime(long long);
	MyTime(std::string timeAsString);
	~MyTime();
	MyTime(const MyTime &t);
	MyTime &operator=(const MyTime &t);
	bool operator>(const MyTime &t);
};


#endif
 /*
 *  MyTimeImplementation.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#include "MyTime.h"

MyTime::MyTime(long long) { }
MyTime::MyTime(std::string timeAsString) { }
MyTime::~MyTime() { }
MyTime::MyTime(const MyTime &t) { }
MyTime &MyTime::operator=(const MyTime &t) { return *this; }
bool MyTime::operator>(const MyTime &t) { return true; }
```

## <a name="Context">[Context](#Context)

```cpp
/*
 *  Context.h
 *  AgileBook
 *
 *  Created by James Coplien on 1/14/09.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _CONTEXT_H
#define _CONTEXT_H

class Context {
public:
	Context(void);
	virtual ~Context();
public:
	static Context *currentContext_;
private:
	Context *parentContext_;
};
 /*
 *  Context.cpp
 *  AgileBook
 *
 *  Created by James Coplien on 1/14/09.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#include "Context.h"

Context::Context(void) {
	parentContext_ = currentContext_;
	currentContext_ = this;
}

Context::~Context() {
	currentContext_ = parentContext_;
}

Context *Context::currentContext_ = NULL;

#endif
```

```cpp
/*
 *  TransferMoneyContext.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/13/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */
 
// TransferMoneyContext knows how to find the objects for a given
// use case invocation; it associates those objects with
// the roles they play in a Use Case of this type; and it
// publishes those interface bindings for use by the
// method-ful roles that participate in the use case.
 
#ifndef _XFERMONEYCONTEXT_H
#define _XFERMONEYCONTEXT_H

#include "Account.h"
#include "Context.h"
#include "Currency.h"
class MoneySource;
class MoneySink;

class TransferMoneyContext: public Context
{
public:
	TransferMoneyContext(void);
	TransferMoneyContext(Currency amount, MoneySource *src, MoneySink *destination);
	void doit(void);
	MoneySource *sourceAccount(void) const;
	MoneySink *destinationAccount(void) const;
	Currency amount(void) const;
private:
	void lookupBindings(void);
	MoneySource *sourceAccount_;
	MoneySink *destinationAccount_;
	Currency amount_;
};
 /*
 *  TransferMoneyContext.cpp
 *  AgileBook
 *
 *  Created by James Coplien on 9/13/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#include "TransferMoneyContext.h"
#include "Currency.h"
#include "MoneySource.h"
#include "MoneySink.h"
#include "InvestmentAccount.h"
#include "SavingsAccount.h"

TransferMoneyContext::TransferMoneyContext(void): Context()
{
	lookupBindings();
}

TransferMoneyContext::TransferMoneyContext(Currency amount, MoneySource *source, MoneySink *destination):
	Context()
{
	// Copy the rest of the stuff
	sourceAccount_ = source;
	destinationAccount_ = destination;
	amount_ = amount;
}

void
TransferMoneyContext::doit(void)
{
	sourceAccount()->transferTo(amount());
}

void
TransferMoneyContext::lookupBindings(void)
{
	// These are somewhat arbitrary and for illustrative
	// purposes. The simulate a database lookup
	InvestmentAccount *investmentAccount = new InvestmentAccount;
	investmentAccount->increaseBalance(Euro(100.00));	// prime it with some money

	sourceAccount_ = investmentAccount;
	destinationAccount_ = new SavingsAccount;
	destinationAccount_->increaseBalance(Euro(500.00));	// start it off with money
	amount_ = Euro(30.00);
}

MoneySource*
TransferMoneyContext::sourceAccount(void) const
{
	return sourceAccount_;
}

MoneySink*
TransferMoneyContext::destinationAccount(void) const
{
	return destinationAccount_;
}

Currency
TransferMoneyContext::amount(void) const
{
	return amount_;
}
 
#endif
```

```cpp
/*
 *  PayBillsContext.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/13/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */
 
 
#ifndef _PAYBILLSCONTEXT_H
#define _PAYBILLSCONTEXT_H

#include "Account.h"
#include "Currency.h"
#include "Context.h"
#include <list>

class MoneySource;
class Creditor;

class PayBillsContext: public Context
{
public:
	PayBillsContext(void);
	void doit(void);
	MoneySource *sourceAccount(void) const;
	std::list creditors(void) const;
private:
	void lookupBindings(void);
	MoneySource *sourceAccount_;
	std::list creditors_;
};
 /*
 *  PayBillsContext.cpp
 *  AgileBook
 *
 *  Created by James Coplien on 9/17/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#include "PayBillsContext.h"
#include "MoneySource.h"
#include "MoneySink.h"
#include "InvestmentAccount.h"
#include "SavingsAccount.h"
#include "Creditor.h"

PayBillsContext::PayBillsContext(void): Context()
{
	lookupBindings();
}

void
PayBillsContext::doit(void)
{
	sourceAccount()->payBills();
}

void
PayBillsContext::lookupBindings(void)
{
	// These are somewhat arbitrary and for illustrative
	// purposes. The simulate a database lookup
	InvestmentAccount *investmentAccount = new InvestmentAccount;
	investmentAccount->increaseBalance(Euro(100.00));	// prime it with some money
	sourceAccount_ = investmentAccount;

	creditors_.push_back(new ElectricCompany);
	creditors_.push_back(new GasCompany);
}

MoneySource*
PayBillsContext::sourceAccount(void) const
{
	return sourceAccount_;
}

std::list
PayBillsContext::creditors(void) const
{
	return creditors_;
}

 
#endif
```

## <a name="Roles">[Roles](#Roles)

```cpp
/*
 *  MoneySource.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _MONEYSOURCE_H
#define _MONEYSOURCE_H

#include "MyTime.h"
#include <string>
#include <list>
using namespace std;

class MoneySink;
class Creditor;
class PayBillsContext;
class TransferMoneyContext;


class MoneySource {
public:
	virtual void transferTo(Currency amount) = 0;
	virtual void decreaseBalance(Currency amount) = 0;
	virtual void payBills(void) = 0;
};

#endif
```

```cpp
/*
 *  MoneySink.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _MONEYSINK_H
#define _MONEYSINK_H

#include <iostream>
#include <string>
#include "MyTime.h"
#include "Currency.h"

using namespace std;

class MoneySource;

class MoneySink {
public:
	virtual void increaseBalance(Currency amount) = 0;
	virtual void updateLog(string, MyTime, Currency amount) = 0;
};

#endif
```

```cpp
/*
 *  TransferMoneySource.h
 *  AgileBook
 *
 *  Created by James Coplien on 9/2/08.
 *  Copyright 2011 Gertrud & Cope. All rights reserved.
 *
 */

#ifndef _TRANSFERFUNDS_H
#define _TRANSFERFUNDS_H

#include "Currency.h"
#include "MoneySource.h"
#include "MoneySink.h"
#include "Creditor.h"
#include "MyExceptions.h"
#include "Globals.h"
#include "PayBillsContext.h"
#include "TransferMoneyContext.h"
#include <string>
#include <list>

#define SELF static_cast<const ConcreteDerived*>(this)

#define RECIPIENT (((dynamic_cast<TransferMoneyContext*>(Context::currentContext_)?	\ 
			dynamic_cast<TransferMoneyContext*>(Context::currentContext_):	\ 
			(throw("dynamic cast failed"), static_cast<TransferMoneyContext*>(NULL))	\ 
			)->destinationAccount()))

// #define RECIPIENT ((static_cast<TransferMoneyContext*>(Context::currentContext_)->destinationAccount()))

#define CREDITORS ((static_cast<PayBillsContext*>(Context::currentContext_)->creditors()))

using namespace std;

template <class ConcreteDerived>
class TransferMoneySource: public MoneySource
{

public:
	// Role behaviors
	void payBills(void) {
		// While object contexts are changing, we don't want to
		// have an open iterator on an external object. Make a
		// local copy.
		std::list creditors = CREDITORS;
		std::list::iterator iter = creditors.begin();
		for (; iter != creditors.end(); iter++ ) {
			try {
				// Note that here we invoke another Use Case
				TransferMoneyContext transferTheFunds((*iter)->amountOwed(),
																SELF,
																(*iter)->account());
				transferTheFunds.doit();
			} catch (InsufficientFunds) {
				throw;
			}
		}
	}

	void transferTo(Currency amount) {
		// This code is reviewable and
		// meaningfully testable with stubs!
		beginTransaction();
		if (SELF->availableBalance() < amount) {
			endTransaction();
			throw InsufficientFunds();
		} else {
			SELF->decreaseBalance(amount);
			RECIPIENT->increaseBalance(amount);
			SELF->updateLog("Transfer Out", DateTime(), amount);
			RECIPIENT->updateLog("Transfer In", DateTime(), amount);
		}
		gui->displayScreen(SUCCESS_DEPOSIT_SCREEN)
		endTransaction();
	}
public:
	TransferMoneySource(void)  { }
};

template <class ConcreteDerived>
class TransferMoneySink: public MoneySink
{
public:
	void transferFrom(Currency amount) {
		SELF->increaseBalance(amount);
		SELF->updateLog("Transfer in", DateTime(), amount);
	}

public:
	TransferMoneySink(void) {
	}

};

#endif
```

## <a name="Main">[Main](#Main)

```cpp
//
//  main.cpp
//  AgileBook
//
//  Created by James Coplien on 9/2/08.
//  Copyright Gertrud & Cope 2011. All rights reserved.
//

#include "TransferMoneyContext.h"
#include "PayBillsContext.h"

int main() {
	TransferMoneyContext *aNewUseCase = new TransferMoneyContext;
	aNewUseCase->doit();
	delete aNewUseCase;
	
	PayBillsContext *anotherNewUseCase = new PayBillsContext;
	anotherNewUseCase->doit();
	delete anotherNewUseCase;
}
```
