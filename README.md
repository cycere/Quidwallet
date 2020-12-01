QUID WALLET BY CYCERE DEVELOPERS.

https://cycere.org

## What is Quidwallet?
----------------

Quidwallet is an experimental digital currency wallet that enables instant payments to
anyone, anywhere in the world based on CRYPTONOTE. Quidwallet uses peer-to-peer 
technology created by Quid Core software to operate and to confirm any type 
of transaction done on the network and through the network worldwaide
with no central authority: managing transactions and issuing money are carried
out collectively by the network. Quid Core wallet is the name of open source
software which enables the use of this currency.


## BUILDING ON UNIX/WINDOWS 

**1. Clone wallet sources**

## QUID SOURCE || LATEST RELEASE
```
git clone https://github.com/cycere/quid.git
```
## QUIDWALLET SOURCE || LATEST RELEASE

```
git clone https://github.com/cycere/quidwallet.git
```

**2. Set symbolic link to coin sources at the same level as `src`. For example:**

```
ln -s ../quid quid
```

Alternative way is to create git submodule:

```
git submodule add https://github.com/cycere/quid.git quid
```

Replace URL with git remote repository of your coin.

**4. Build**

```
mkdir build && cd build && cmake .. && make
```

Good Luck and enjoy
