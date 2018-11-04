%% Forritunarverkefni
close all
clear all
clc

Föllin okkar:
Stimuli fallið
function [stnum y]=stimulifall(B)
%búum til fall sem telur fjölda stimula og reiknar út meðaltímalengd þeirra
x=find(B(:,2)==1);
stnum=length(x);
y=mean(B(x));

fprintf('Heildarfjöldi stimuli er %0.0f \n',stnum)
fprintf('Meðaltími hvers stimulus er %0.2f sekúndur \n',y)
end


Stöðubreyting
function [a,b,c,d]=stodubreyting(x,y,z,r)
%finna út hvernig staðan breytist milli a b c d
%finnum fyrst tölugildi af stærðinni og svo meðaltalið af henni
a=abs(x);
a=mean(a);

b=abs(y);
b=mean(b);

c=abs(z);
c=mean(c);

d=abs(r);
d=mean(d);
end
...................................................

close all;
clear all;
clc;

%% Forritunarverkefni
close all;
clear all;
clc;

slodi=uigetdir('veldu möppu');  %náum í folderinn sem þarf
%búum til forlykkjur til að hlaða inn hverri skrá fyrir sig
%skrár fyrir lokuð augu
for i=1:33
    skra=strcat('SUB',num2str(i),'_closed','.xlsx');
    s=strcat(slodi,'\',skra); 
    A(i).closed=xlsread(s); %söfnum gögnum inn í structure breytu
end
%skrár fyrir opin augu
for i=1:33
    skra=strcat('SUB',num2str(i),'_open','.xlsx');  
    s=strcat(slodi,'\',skra);
    A(i).open=xlsread(s);
end
%stimuli skráin sótt
skra=strcat('Stimuli','.xlsx');
s=strcat(slodi,'\',skra);
B=xlsread(s);   %skýrum breytuna B



%%
%Veljum annan dálkinn i breytu B sem inniheldur stimuli skrána
%Breytum tíðninni þannig að 20 Hz taki gildið 0 og 59 Hz taki gildið 1

Tidni=B(:,2);

for i=1:length(Tidni)
    if Tidni(i)==20
        Tidni(i)=0;
    elseif Tidni(i)==59
        Tidni(i)=1;
    end  
end
B(:,2)=Tidni; %breytum dálk 2 í nýju gildin úr for-lykkjunni

%Bjuggum til fall til að telja fjölda stimuli og reiknar út meðaltímalengd
%þeirra

hfjoldi=stimulifall(B);
%fallið prentar út niðurstöðu í command window

%búum til möppu til að vista gröfin okkar úr niðurstöðum fyrir hvern
%einstakling
mkdir myndir

%gerum for lykkju fyrir alla 33 einstaklingana og teiknum upp gröfin

for i=1:33
    figure(i)
    
    %setjum fyrst inn stimuli hlutann
    subplot(3,2,1)
    plot(B(1:1500,1),B(1:1500,2),'m',B(1501:5250,1),B(1501:5250,2),'r',...
        B(5251:9000,1),B(5251:9000,2),'g',B(9001:12750,1),B(9001:12750,2),'b',...
        B(12751:16500,1),B(12751:16500,2),'k')
    hold
    axis([0 330 -10 10])
    yticklabels({''})   %viljum taka út y ásinn, hafa hann tóman
    title('Stimuli','FontSize',7.3)
    
    %Stimuli mynd nr 2
    subplot(3,2,2)
    plot(B(1:1500,1),B(1:1500,2),'m',B(1501:5250,1),B(1501:5250,2),'r',...
        B(5251:9000,1),B(5251:9000,2),'g',B(9001:12750,1),B(9001:12750,2),'b',...
        B(12751:16500,1),B(12751:16500,2),'k')
    hold
    axis([0 330 -10 10])
    yticklabels({''})
   title('Stimuli','FontSize',7.3) 
   
    %Sækjum núna skránna fyrir opin augu og setjum það upp í graf
    x=A(i).open;
    Qs=x(1:1500,:); %gerum fyrir hvern einasta Q hluta, þar sem Qs er quiet stance.
    %veljum ákveðin stök í file-num til að aðgreina Q hlutana
    Q1=x(1501:5250,:);
    Q2=x(5251:9000,:);
    Q3=x(9001:12750,:);
    Q4=x(12751:16500,:);
    subplot(3,2,3)
    plot(Qs(:,1),Qs(:,2),'m',Q1(:,1),Q1(:,2),'r',...
        Q2(:,1),Q2(:,2),'g', Q3(:,1),Q3(:,2),'b',Q4(:,1),Q4(:,2),'k')
    hold
    axis([0 330 -40 40])
    title('Opin augu: Medial/Lateral vægi','LineWidth',3) %setjum inn texta á gröfin
    ylabel('Torque [Nm]')
    
    x=A(i).closed;  %sótt skránna fyrir lokuð augu
    Qs=x(1:1500,:); 
    Q1=x(1501:5250,:);
    Q2=x(5251:9000,:);
    Q3=x(9001:12750,:);
    Q4=x(12751:16500,:);
    subplot(3,2,4)
    plot(Qs(:,1),Qs(:,2),'m',Q1(:,1),Q1(:,2),'r',...
        Q2(:,1),Q2(:,2),'g', Q3(:,1),Q3(:,2),'b',Q4(:,1),Q4(:,2),'k')
    axis([0 330 -40 40])
    title('Lokuð augu: Medial/Lateral vægi','LineWidth',3)
    ylabel('Torque [Nm]')
    
    %Bætum við Anterior/posterior vægi við opin augu
    x=A(i).open;
    Qs=x(1:1500,:); %gerum fyrir hvert einasta Q
    Q1=x(1501:5250,:);
    Q2=x(5251:9000,:);
    Q3=x(9001:12750,:);
    Q4=x(12751:16500,:);
    subplot(3,2,5)
    plot(Qs(:,1),Qs(:,3),'m',Q1(:,1),Q1(:,3),'r',...
        Q2(:,1),Q2(:,3),'g', Q3(:,1),Q3(:,3),'b',Q4(:,1),Q4(:,3),'k')
    hold
    axis([0 330 -40 40])
    title('Opin augu: Anterior/posterior vægi','LineWidth',3)
    ylabel('Torque [Nm]')
    xlabel('Tími[s]')
    
    x=A(i).closed; %lokuð augu
    Qs=x(1:1500,:); 
    Q1=x(1501:5250,:);
    Q2=x(5251:9000,:);
    Q3=x(9001:12750,:);
    Q4=x(12751:16500,:);
    subplot(3,2,6)
    plot(Qs(:,1),Qs(:,3),'m',Q1(:,1),Q1(:,3),'r',...
        Q2(:,1),Q2(:,3),'g', Q3(:,1),Q3(:,3),'b',Q4(:,1),Q4(:,3),'k')
    axis([0 330 -40 40])
    title('Lokuð augu: Anterior/posterior vægi','LineWidth',3)
    ylabel('Torque [Nm]')
    xlabel('Tími[s]')
    %setjum inn örvar til að útskýra hvern Q hlut
    x = [0.15 0.15]; %ef þú breytir þessum tölum færist örin til hliðar
    y = [0.15 0.2]; % ef þú breytir þessum breytist lengdin á örinni
    annotation('textarrow',x,y,'String','QS ')

    x1 = [0.2 0.2];
    y1 = [0.15 0.2];
    annotation('textarrow',x1,y1,'String','Q1 ')

    x2 = [0.27 0.27];
    y2 = [0.15 0.2];
    annotation('textarrow',x2,y2,'String','Q2 ')

    x3 = [0.35 0.35];
    y3 = [0.15 0.2];
    annotation('textarrow',x3,y3,'String','Q3 ')

    x4 = [0.43 0.43];
    y4 = [0.15 0.2];
    annotation('textarrow',x4,y4,'String','Q4 ')

    %vistum myndirnar af gröfunum í tölvunni eina í einu
    cd(strcat(pwd,'\myndir')) %köllum í folderinn
    temp=['fig',num2str(i),'.png']; %þessi skipun skýrir hverja mynd fyrir sig
    saveas(gca,temp); 
    cd .. %förum út úr möppunni 'myndir' og aftur í okkar möppu
end


%búum til aðra for-lykkju fyrir hvern einstakling þar sem tekinn er munur
%á plönunum tveimur með lokuð og opin augu

for i=1:33
    figure
    munur=A(i).open;
    subplot(1,2,1)
    plot(munur(:,2),munur(:,3),'b.','LineWidth',0.3)
    axis([-15 15 -20 30])
    title('Opin augu')
    xlabel('Medial/lateral [Nm]')
    ylabel('Anterior/Posterior [Nm]')
    
    mun=A(i).closed;
    subplot(1,2,2)
    plot(mun(:,2),mun(:,3),'b.','LineWidth',0.3)
    axis([-15 15 -20 30])
    title('Lokuð augu')
    xlabel('Medial/lateral [Nm]')
    ylabel('Anterior/Posterior [Nm]')
    

end

%viljum finna hvernig staðan breytist milli Q1,Q2,Q3 og Q4 og hvort fólki 
%takist að læra inná það hvernig bregðast skuli við örvunum.

%breytan br finnur Qs,Q1,Q2,Q3 og Q4 fyrir alla einstaklingana
%stór stafur fyrir opin augu, lítill fyrir lokuð
br=A.closed;
%gerum fyrir alla hluta Q nema Qs
    Q1=br(1501:5250,:); 
    Q2=br(5251:9000,:);
    Q3=br(9001:12750,:);
    Q4=br(12751:16500,:);
    
bre=A.open; %opin augu
    q1=bre(1501:5250,:);
    q2=bre(5251:9000,:);
    q3=bre(9001:12750,:);
    q4=bre(12751:16500,:);
    
 %viljum finna hvernig staðan breytist milli Q1,Q2,Q3 og Q4 með því að
 %finna meðaltal af tölugildi (abs)
%bjuggum til fall sem gerir það, köllum í það:

[Qq1,Qq2,Qq3,Qq4]=stodubreyting(Q1,Q2,Q3,Q4); %opin augu

%höfum bara áhuga á dálk 2 og 3 sem segja okkur kraftvægið á lateral og
%posterior

Qq1=Qq1(:,2:3);
Qq2=Qq2(:,2:3);
Qq3=Qq3(:,2:3);
Qq4=Qq4(:,2:3);

disp('frammistaða með opin augu') %Skrifum á skjáinn mat okkar á frammistöðu

%búum til if setningu sem skrifar út niðurstöðu hvort frammistaðan varð
%betri eða verri með tímanum.
if (Qq2(:,1)+Qq2(:,2))<(Qq1(:,2)+Qq1(:,1))
    disp('Staðan lækkar milli Q1 og Q2, frammistaða betri')
else
    disp('Staðan hækkar milli Q1 og Q2, frammistaða verri')
end
if (Qq3(:,1)+Qq3(:,2))<(Qq2(:,2)+Qq2(:,1))
    disp('staðan lækkar milli Q2 og Q3, frammistaða betri')
else
    disp('Staðan hækkar milli Q2 og Q3, frammistaða verri')
end
if (Qq4(:,1)+Qq4(:,2))<(Qq3(:,2)+Qq3(:,1))
    disp('Staðan lækkar milli Q3 og Q4, frammistaða betri')
else
    disp('Staðan hækkar milli Q3 og Q4, frammistaða verri')
end


%köllum núna í fallið til að finna stöðubreytingu, fyrir lokuð augu

[qq1,qq2,qq3,qq4]=stodubreyting(q1,q2,q3,q4);

qq1=qq1(:,2:3); %höfum bara áhuga á dálkum 2 og 3 sem eru hliðlæg og framogaftur
qq2=qq2(:,2:3);
qq3=qq3(:,2:3);
qq4=qq4(:,2:3);

disp('frammistaða með lokuð augu')
if (qq2(:,1)+qq2(:,2))<(qq1(:,2)+qq1(:,1))
    disp('Staðan lækkar milli Q1 og Q2, frammistaða betri')
else
    disp('Staðan hækkar milli Q1 og Q2, frammistaða verri')
end
if (qq3(:,1)+qq3(:,2))<(qq2(:,2)+qq2(:,1))
    disp('staðan lækkar milli Q2 og Q3, frammistaða betri')
else
    disp('Staðan hækkar milli Q2 og Q3, frammistaða verri')
end
if (qq4(:,1)+qq4(:,2))<(qq3(:,2)+qq3(:,1))
    disp('Staðan lækkar milli Q3 og Q4, frammistaða betri')
else
    disp('Staðan hækkar milli Q3 og Q4, frammistaða verri')
end


% Greinum muninn milli þess að hafa opin augu og lokuð augu.

o=A.open;
o=o(:,2:3); %veljum dálka 2 og 3
o=o(:,1)+o(:,2); %plúsa saman lateral og anterior posterior
o=mean(o);  %finnum meðaltalið af gildunum fyrir opinn augu og svo lokuð.
c=A.closed;
c=c(:,2:3);
c=c(:,1)+c(:,2);
c=mean(c);
munurinn=o-c;   %munurnn er jákvæð tala og er o þess vegna stærri tala
%notum if lykkju til að segja okkur hvort gekk betur
if munurinn>0
    disp('Yfir allt stóðu einstaklingar sig betur með lokuð augu')
else 
    disp('Yfir allt stóðu einstaklingar sig betur með opin augu')
end

%Hver stóð sig best?
%sá sem er með minnsta staðalfrávik frá 0 stóð sig best

for i=1:33 
    x=A(i).open;
    x=x(:,2:3); %veljum dálkana með hliðlægu og framogaftur gildin
    F{i}=std(x); %F er staðalfrávik fyrir opin augu
    y=A(i).closed;
    y=y(:,2:3);
    f{i}=std(y); %f er staðalfrávik fyrir lokuð augu
    
end

%sá sem stóð sig best hefur std næst núlli
for i=1:33
    Stad(i)=min(F{i}(1)+F{i}(2)); %leitað að minnsta gildi yfir allt
    stad(i)=min(f{i}(1)+f{i}(2));
    %summmum saman lateral og anterior/posterior gildunum og 
    %finnum minnsta gildið af því
end

Minnsta=min(Stad);
Bestur=find(Stad==Minnsta); %finnum hvar í breytunni Stad minnsta gildið er
Besturiheimi=Stad(24);

%sama fyrir lokuð augu:
minnsta=min(stad);
bestur=find(stad==minnsta); %finnum hvar í breytunni 'stadð minnsta gildið er
besturiheimi=stad(24); %besta gildið fundið
fprintf('einstaklingur %.f stóð sig best og var hann með gildi %.2f með opin augu og %.2f með lokuð augu \n',Bestur,Besturiheimi,besturiheimi)

%einstaklingur nr 24 stóð sig best

