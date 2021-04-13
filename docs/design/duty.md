> 责任链模式

```
class Account {
  next(account) {
    this.successor = account
  }

  pay(amount) {
    if (this.canPay(amount)) {
      console.log(`Paid ${amount} using ${this.name}`)
    } else if (this.successor) {
      console.log(`Cannot pay using ${this.name}. Proceeding...`)
      this.successor.pay(amount)
    } else {
      console.log('None of the accounts have enough balance')
    }
  }

  canPay(amount) {
    return this.balance >= amount
  }
}

class Bank extends Account {
  constructor(balance) {
      super()
      this.name = 'bank'
      this.balance = balance
  }
}

class Paypal extends Account {
  constructor(balance) {
      super()
      this.name = 'Paypal'
      this.balance = balance
  }
}

class Bitcoin extends Account {
  constructor(balance) {
      super()
      this.name = 'bitcoin'
      this.balance = balance
  }
}

const bank = new Bank(100)          // Bank with balance 100
const paypal = new Paypal(200)      // Paypal with balance 200
const bitcoin = new Bitcoin(300)    // Bitcoin with balance 300

bank.next(paypal)
paypal.next(bitcoin)
```
