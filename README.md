# MCSEXP
exp 2
clc;
clear;
close;
//Inputs
Pt = input("enter the transmitted power:");
Gt = input("enter the transmitted gain of antenna:");
Gr = input("enter the receive gain of antenna:");
f = input("enter the frequency:");
d = input("enter the distance:");
//constant
c=3*10^8;
lambda=c/f;
pr= Pt*Gt*Gr*(lambda/(4*%pi*d))^2;
disp("pr("+string(d)+")="+string(pr));
disp("pr(db)="+string(round(10*log10(pr))));
disp("pr(dbm)="+string(round(10*log10(pr*1000))));
R=1:30;
PL1=91.52+20*log10(R);
PL2=97.54+20*log10(R);
subplot(2,1,1)
plot(R,PL1);
xgrid()
xtitle("path loss for 900MHz","distance(km)","pathloss(db)")
subplot(2,1,2)
plot(R,PL2)
xgrid()
xtitle("path loss for 1800MHz","distance(km)","pathloss(db)")
