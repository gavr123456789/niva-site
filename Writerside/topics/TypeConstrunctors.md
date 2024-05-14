# TypeConstructors

```Scala
constructor Float pi = 3.14
Float pi echo

type Wallet money: Int
constructor Wallet empty = Wallet money: 0
Wallet empty = money <- 0

emptyWallet::mut Wallet = Wallet empty
emptyWallet money echo

emptyWallet money: 20
emptyWallet money echo

emptyWallet empty
emptyWallet money echo
```