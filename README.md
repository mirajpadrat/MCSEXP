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
xtitle("path loss for 900MHz","distance(km)","pathloss(db)")
xgrid()

subplot(2,1,2)
plot(R,PL2)
xtitle("path loss for 1800MHz","distance(km)","pathloss(db)")
xgrid()

exp 3

clc;
clear;

n = 4;
io = 6;
T = [];

for i = 1:5
for j = 0:i
N = i^2 + i*j + j^2;
Q = sqrt(3*N);
CI = 10*log10((Q^n)/io);
T = [T; i j N Q CI];
end
end

T = gsort(T, "lr", "i");

k = 1;
while k < size(T,1)
if T(k,3) == T(k+1,3) then
T(k,:) = [];
else
k = k + 1;
end
end
disp(" i j N Q C/I(dB)");
disp(T);

exp 4 

clc;
clear;
c = 3e8;

v = input("Enter mobile speed Vm (in km/h): ");
fc = input("Enter carrier frequency fc (in Hz): ");
theta = input("Enter angle theta (in degrees or array like [0, 180, 90, 60]): ");

lambda = c/fc;
fd = (v/3.6)/lambda * cosd(theta);
fr = fc + fd;

disp("Car Signal Analysis:");
for i = 1:length(theta)
printf("Angle: %3d deg | Doppler Shift: %8.2f Hz | Received Freq: %11.2f Hz\n", theta(i), fd(i), fr(i));
end

v = 0:5:120;
plot(v, (v/3.6)/lambda);
xtitle("Maximum Doppler Shift vs. Mobile Speed", "Speed (km/h)", "Max Doppler Shift (Hz)");
xgrid();

exp 6

clc;
clear;

s = input("Enter 4-bit initial state (e.g. [1 0 0 0] or 1000): ");
if length(s)==1 then
s = evstr(strsplit(string(s),
""));
end

p = [];
mprintf("\n================ LFSR State Table ================\n");
mprintf("Step\tQ0\tQ1\tQ2\tQ3\tOutput (Q3)\n");
mprintf("--------------------------------------------------\n");

for i = 0:14
p = [p s(4)];
s = [bitxor(s(3),s(4)) s(1:3)];
end

mprintf("%d\t%d\t%d\t%d\t%d\t%d\n",i,s(1),s(2),s(3),s(4),s(4));
mprintf("--------------------------------------------------\n");
mprintf("\nGenerated PN Sequence (15 bits):\n");
disp(p);
mprintf("\n================ Bit Analysis ================\n");
mprintf("Number of 1s: %d\n",sum(p==1));
mprintf("Number of 0s: %d\n",sum(p==0));
mprintf("==============================================\n");
clc;
clear;

s = input("Enter 4-bit initial state (e.g. [1 0 0 0] or 1000): ");
if length(s)==1 then
s = evstr(strsplit(string(s),
""));
end

p = [];
mprintf("\n================ LFSR State Table ================\n");
mprintf("Step\tQ0\tQ1\tQ2\tQ3\tOutput (Q3)\n");
mprintf("--------------------------------------------------\n");

for i = 0:14
p = [p s(4)];
s = [bitxor(s(3),s(4)) s(1:3)];
end

mprintf("%d\t%d\t%d\t%d\t%d\t%d\n",i,s(1),s(2),s(3),s(4),s(4));
mprintf("--------------------------------------------------\n");
mprintf("\nGenerated PN Sequence (15 bits):\n");
disp(p);
mprintf("\n================ Bit Analysis ================\n");
mprintf("Number of 1s: %d\n",sum(p==1));
mprintf("Number of 0s: %d\n",sum(p==0));
mprintf("==============================================\n");

exp 7

clc;
clear;

A = input("Enter Data Bit for Sender A (0 or 1): ");
B = input("Enter Data Bit for Sender B (0 or 1): ");

Ad = 2*A-1;
Bd = 2*B-1;

CA = 2*[0 0 1 1 0 0 1 1]-1;
CB = 2*[0 1 1 0 0 1 1 0]-1;

As = Ad*CA;
Bs = Bd*CB;
Cs = As+Bs;

sum_A = sum(Cs.*CA);
sum_B = sum(Cs.*CB);

rec_A = (sum_A > 0);
rec_B = (sum_B > 0);

disp("--- TRANSMITTER ---");
disp("Bipolar Data Ad:",Ad);
disp("Bipolar Data Bd:",Bd);
disp("Spread Data As:",As);
disp("Spread Data Bs:",Bs);

disp("--- CHANNEL ---");
disp("Combined Signal Cs:",Cs);

disp("--- RECEIVER ---");
disp("Receiver A Sum:",sum_A);
disp("Recovered Data A:",rec_A);
disp("Receiver B Sum:",sum_B);
disp("Recovered Data B:",rec_B);

exp 8 

clc; clear; close;

a = input("Enter input bits (example [1 0 1 0]): ");
n = length(a);
a = [a zeros(1,8)];
N = length(a);

m = zeros(1,9);
v1 = zeros(1,N);
v2 = zeros(1,N);

mprintf("\n==============================================\n");
mprintf(" CONVOLUTIONAL ENCODER\n");
mprintf("==============================================\n");

mprintf("\nOriginal Input : %s",strcat(string(a(1:n))));
mprintf("\nInput with zeros : %s\n",strcat(string(a)));

mprintf("\n==============================================\n");
mprintf(" STATE TABLE\n");
mprintf("==============================================\n");
mprintf("Step Input Present State V1 V2 Next State\n");
mprintf("------------------------------------------------\n");

for j = 1:N
p = strcat(string(m(1:8)));

v1(j) = modulo(sum(m([1 2 3 4 6 8 9])),2);
v2(j) = modulo(sum(m([1 3 4 5 9])),2);

m = [a(j) m(1:8)];
q = strcat(string(m(1:8)));

mprintf("%-5d %-6d %-14s %-3d %-3d %s\n", ...
j,a(j),p,v1(j),v2(j),q);
end

mprintf("================================================\n");
mprintf(" OUTPUT\n");
mprintf("================================================\n");

s1 = strcat(string(v1));
s2 = strcat(string(v2));


s = “”;
for i = 1:N
s = s + string(v1(i)) + string(v2(i));
end 

mprintf("\nV1 = %s\nV2 = %s",s1,s2);
mprintf("\n\nFinal Encoded Output = %s\n",s);

mprintf("\n==============================================\n");
mprintf(" CONVOLUTIONAL ENCODING COMPLETED\n");
mprintf("==============================================\n");

exp 9 

clc; clear;
n = input("Enter the order of Hadamard matrix: ");

H = 1;
while size(H,1) < n
H = [H H; H -H];
end

disp("Hadamard Matrix:");
disp(H);

for i = 1:n
changes(i) = sum(H(i,2:n) <> H(i,1:n-1));
end

[s,index] = gsort(changes,"g","i");

disp("Walsh Codes:");
disp(H(index,:));

