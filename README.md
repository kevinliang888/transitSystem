﻿# Transit System
A simulation of real life transit system.

## Getting Started
When checking the code, you may find some “Cannot resolve” symbols, that’s because you haven’t
imported the external libraries we used. The path of external libraries is: /transitSystem/libs

This is a simple guide to show you how to import them to IntelliJ IDEA
1. Open IntelliJ IDEA, click File -> Project Structure
2. Click Modules -> Dependencies -> small “+” button -> JARS or directories…
3. Select two .jar files in libs (path: /transitSystem/libs), then click Open/OK.
4. Check two external libraries, then click OK.

## Instruction
1. You can simply run the Main function from source code on IDEs, or download the packed app [here](https://github.com/kevinliang888/transitSystem/releases). Note that configuration.txt has to be in the same directory.
2. The system is initialized to be closed. Therefore administrator has to login his/her account to open the system. The account email and password are initialized to be "admin".
3.  Then you can apply for a new card directly. There are three different kind of transit pass to choose:  Traffic Card, 10 times Pass, Weekly Pass. After applying for a new card, you can use the card to tap in and tap out the station. You can also check the balance and top up the card. 
4.  You can create a new account to manage all of your cards and track your card activity. 


## Prerequisites
This program only works with JDK 1.8

## Design
Observer:
1. It is used in WeeklyPass to observe the system time that modified in adminUser, since they are
not related, to avoid coupling and make sure each WeeklyPass is able to get the current system
time immediately.
2. It is used in MVC design pattern. Firstly, every time when user taps in or out, the status label
will be modified by observing the changes in transitManager. Secondly, if user edit his user name
successfully, the label(view) which represents the user name will be modified by observing the
changes in accountManager.

Iterator:
To delete data at a specified date while iterating.

Strategy:
The system requires different strategies while deducting money.

Serialize:
To save all the information of this system(stations, trips, revenues) and customer(cards and accounts)

Factory:
When create different kinds of cards that inherit from TransitPass.

Logging:
To save all the system information that involved instance change(INFO LEVEL) and Exception
raised(WARING or SEVERE), and some operations(FINE) for debugging.

Singleton:
Since the inquiries require this system can only generate one log file "log.txt", we compared some
implementations, it will result in all the class calling AdminUser often if we define a static log
in AdminUser. Implementing different log instances in different managers will cause java to generate
different log files, ex: "log.txt.1", "log.txt.2", etc. Therefore, we decided to use Singleton to
implement the log.

• This design also follows all of five SOLID principles.

1. Single responsibility principle
All the operations involved in content change on this system are separated from three Managers.
For report generating functions and view information functions, we are able to use each specified
instance's function such as isOwingMoney and toString in transitPass, and view information functions
are all done by admin.

2. Open closed principle
This system is open since it is still available for further extension, all functions are specified
and separately placed in each class, you are welcomed to add any new features by simply add new
class or strategy, or add if statements in top module. This system is closed since we don't need to
modify some base functions while adding new features.

3. Liskov substitution principle
We followed this principle while defining cards class, all cards under TransitPass can be replaced
by each other, and we also have AbleTopUp interface, all cards under this interface are free to
substitute by each other.

4. Interface segregation principle
Since some cards need topUp function while some are not, we created AbleTopUp interface for those
cards. Since there are different types of stations: "subway station" and "bus stop", and the fare
calculation is different, so we use FareStrategy interface to calculate fare for different type of
stations.

5. Dependency inversion principle
When we implementing TransitPass superclass, we avoid implementing top up function and all cards
under this class are only able to record information and deduct, while all other methods are for
visiting information, but not for change content. (also applied single responsibility principle here)

