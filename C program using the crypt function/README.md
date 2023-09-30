### Create your own C program using the crypt function, and compile this into a command.
```
cat MyCrypt.c
```
```
#include <stdio.h> 
#define USE_XOPEN 
#include <unistd.h>
int main(int argc, char** argv)
{
if(argc==3)
{
printf("%s\n", crypt(argv[1],argv[2]));
}
else
{
printf("Usage: MyCrypt $password $salt\n" );
}
return 0;
}
```
### Compile MyCrypt.c file 
```
gcc MyCrypt.c -o MyCrypt -lcrypt
```
[NOTE] If gcc is not installed you need to first install it.
```
yum install gcc -y
```
### To use it, we need to give two parameters to MyCrypt. The first is the unencrypted password, the second is the salt. The salt is used to perturb the encryption algorithm in one of 4096 different ways. This variation prevents two users with the same password from having the same entry in /etc/shadow.
```
./MyCrypt hunter2 42
```
```
./MyCrypt hunter2 33
```
[NOTE] You'll notice that the first two characters of the password are the salt.

### The standard output of the crypt function is using the DES algorithm which is old and can be cracked in minutes. A better method is to use md5 passwords which can be recognized by a salt starting with $1$.
```
 ./MyCrypt hunter2 '$1$42'
```
```
 ./MyCrypt hunter2 '$1$42'
```

[NOTE] The md5 salt can be up to eight characterslong. The salt is displayed in /etc/shadow between the second and third $, so never use the password as the salt!

```
./MyCrypt hunter2 '$1$hunter2'
```
