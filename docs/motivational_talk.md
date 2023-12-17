Building my own Linux distro in this case probably





Particularly doing so without using any hacks
linux from scratch has a couple of places where they say "and now use sed to change this" and I wanted to not do that
brunoa
Bruno Alves de Moura Chaves @brunoa
14:35
did you integrate grub?
hp
Hein-Pieter van Braam @hp
Admin
14:35
So I spent a lot of time figuring out how it should be done, basically
brunoa
Bruno Alves de Moura Chaves @brunoa
14:35
I see
so, no hacks, no skipping steps...
hp
Hein-Pieter van Braam @hp
Admin
14:35
Also, I didn't really use any documentation other than what I had learned through osmosis what a minimal linux consists of
And then just read the actual project documentation
brunoa
Bruno Alves de Moura Chaves @brunoa
14:36
but, you used thirdparty modules, right?
hp
Hein-Pieter van Braam @hp
Admin
14:36
But I had done an LFS using their documentation a couple of years before that
thirdparty modules?
brunoa
Bruno Alves de Moura Chaves @brunoa
14:36
Oh, so you built literally using LINUX FROM SCRATCH THE BOOK...
hp
Hein-Pieter van Braam @hp
Admin
14:36
yeah
well, yes and no
I did that once, wasn't happy with the amount of hackery, and started over without looking at it
let me see if that hackery is still in there
brunoa
Bruno Alves de Moura Chaves @brunoa
14:37
But, you used OSDev at least, no?
hp
Hein-Pieter van Braam @hp
Admin
14:38
yeah, it's still there:
cd ..
cat gcc/limitx.h gcc/glimits.h gcc/limity.h > \
  `dirname $($LFS_TGT-gcc -print-libgcc-file-name)`/include/limits.h
stuff like this
brunoa
Bruno Alves de Moura Chaves @brunoa
14:38
I am in awe if you did all of that with basically a terminal and manpages(plus your previous knowledgment of linux)
hp
Hein-Pieter van Braam @hp
Admin
14:38
Well, I used online docs too 😛 But only of the project
brunoa
Bruno Alves de Moura Chaves @brunoa
14:39
which project to be exact?
hp
Hein-Pieter van Braam @hp
Admin
14:39
oh, whatever one I was building
brunoa
Bruno Alves de Moura Chaves @brunoa
14:39
oh, so it was a series or something?
ok
I see
hp
Hein-Pieter van Braam @hp
Admin
14:39
So, gcc, binutils, glibc, linux, coreutils, etc
figuring out one by one what I even needed
Step 1 is basically making a "self contained" gcc with a proper sysroot
then you use that to build glibc, then gcc again with that new glibc
it's a bit of an oroborus there's some stuff you have to build more than once
and you need to know the difference between a prefix and a destdir and a sysroot
brunoa
Bruno Alves de Moura Chaves @brunoa
14:40
how did you manage that? make?
hp
Hein-Pieter van Braam @hp
Admin
14:41
well, make is certainly an important part of it 😛
so, you're building gcc stage 2 for instance
so you ./configure --prefix=/usr but you don't want to overwite your own
so you end up doing make
but the make install DESTDIR=/path/to/your/new/distro
for instance
and you need to work out how to do that with a wide variety of buildsystems
and make sure you never accidentally end up using your "normal" gcc
brunoa
Bruno Alves de Moura Chaves @brunoa
14:42
hahaha
I can see myself destroying my machine pretty easily if I was trying that some years ago...
hp
Hein-Pieter van Braam @hp
Admin
14:42
well, that's why when you're doing this you never ever use sudo 😛
that's a bit of a safety rail you can give yourself
but basically my goal was to get to a chroot'able environment where I could build the rest asap
brunoa
Bruno Alves de Moura Chaves @brunoa
14:43
and what did you put on your own distro?
hp
Hein-Pieter van Braam @hp
Admin
14:43
so I think I just had bash, glibc, coreutils, awk, make, gcc in there to start
and then from there I built everything else
brunoa
Bruno Alves de Moura Chaves @brunoa
14:44
dang...
hp
Hein-Pieter van Braam @hp
Admin
14:44
Once you're in your chroot it becomes a lot easier
brunoa
Bruno Alves de Moura Chaves @brunoa
14:44
everything makes sense, how you were able to do the toolchain for godot
hp
Hein-Pieter van Braam @hp
Admin
14:44
that was far, far easier
brunoa
Bruno Alves de Moura Chaves @brunoa
14:44
I mean, buildroot gives us a nice framework, but it is still complex af
hp
Hein-Pieter van Braam @hp
Admin
14:44
I used an off the shelf system for that 😛
The original windows version was basically a "linux from scratch" though
brunoa
Bruno Alves de Moura Chaves @brunoa
14:44
yeah, for someone with your expertise it is far far easier
ok, gonna copy this chat if you don´t mind
hp
Hein-Pieter van Braam @hp
Admin
14:45
Sure
I don't think I said anything too wild 😛
brunoa
Bruno Alves de Moura Chaves @brunoa
14:45
and it will go to my notes for a weekend project, that will probably take 3 months
hp
Hein-Pieter van Braam @hp
Admin
14:45
I mean, you can start small
brunoa
Bruno Alves de Moura Chaves @brunoa
14:45
Yeah, I will be pretty happy with just the first part...
hp
Hein-Pieter van Braam @hp
Admin
14:46
figure out the minimum amount of shit you need to build to make chroot /path/to/your/stuff to work and actually be able to type 'ls'
that won't be good, and you will run into problems almost immediately
brunoa
Bruno Alves de Moura Chaves @brunoa
14:46
having gcc and glibc
hp
Hein-Pieter van Braam @hp
Admin
14:46
but if you have that then you've built something at least
na'h, for that you really only need glibc bash and coreutils
you can even use your system compiler
It's a good start to practice
And simple things to poke at
