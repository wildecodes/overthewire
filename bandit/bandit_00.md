The first step is to simply ssh into the game.

ssh -p 2220 bandit0@bandit.labs.overthewire.org
password: bandit0

for future reference, if it is level n, the user is banditn @ bandit.labs.overthewire.org

or more generally, [game(n)] @ [game].labs.overthewire.org

I thought setting the port number in ssh could be done like 
bandit0@bandit.labs.overthewire.org:2220 ( :2220 after hostname ) but for some 
reason this didn't work. Maybe because I am on a mac, and it's a linux thing? 
Setting the port number by passing it as an argument to the -p flag works.
