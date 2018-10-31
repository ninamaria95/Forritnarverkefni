%% Forritunarverkefni
close all;
clear all;
clc;

slodi=uigetdir('veldu möppu')

for i=1:33
    skra=strcat('SUB',num2str(i),'_closed','.xlsx');
    s=strcat(slodi,'\',skra);
    A(i).closed=xlsread(s);
end

for i=1:33
    skra=strcat('SUB',num2str(i),'_open','.xlsx');
    s=strcat(slodi,'\',skra);
    A(i).open=xlsread(s);
end

skra=strcat('Stimuli','.xlsx');
s=strcat(slodi,'\',skra);
B=xlsread(s);



%%

Tidni=B(:,2);

for i=1:length(Tidni)
    if Tidni(i)==20
        Tidni(i)=0;
    elseif Tidni(i)==59
        Tidni(i)=1;
    end  
end
B(:,2)=Tidni;

%%
%köllum í fallið okkar

hfjoldi=stimulifall(B);

%% 

%gerum for lykkju fyrir alla 33 einstaklingana

for i=1:2
    figure(i)
  
    %núna setjum við inn stimuli hlutann
    subplot(3,2,1)
    plot(B(1:1500,1),B(1:1500,2),'m',B(1501:5250,1),B(1501:5250,2),'r',...
        B(5251:9000,1),B(5251:9000,2),'g',B(9001:12750,1),B(9001:12750,2),'b',...
        B(12751:16500,1),B(12751:16500,2),'k')
    hold
    axis([0 330 0 100])
%     set(gca,'XTickLabel',[0:330])
%     set(gca,'YTickLabel',[-5 5])
    title('Stimuli','FontSize',7.3)
    
    %Stimuli mynd nr 2
    subplot(3,2,2)
    plot(B(1:1500,1),B(1:1500,2),'m',B(1501:5250,1),B(1501:5250,2),'r',...
        B(5251:9000,1),B(5251:9000,2),'g',B(9001:12750,1),B(9001:12750,2),'b',...
        B(12751:16500,1),B(12751:16500,2),'k')
    hold
    axis([0 330 0 100])
   title('Stimuli','FontSize',7.3) 
   
   
    x=A(i).open;
    Qs=x(1:1500,:); %gerum fyrir hvert einasta Q
    Q1=x(1501:5250,:);
    Q2=x(5251:9000,:);
    Q3=x(9001:12750,:);
    Q4=x(12751:16500,:);
    subplot(3,2,3)
    plot(Qs(:,1),Qs(:,2),'m',Q1(:,1),Q1(:,2),'r',...
        Q2(:,1),Q2(:,2),'g', Q3(:,1),Q3(:,2),'b',Q4(:,1),Q4(:,2),'k')
    hold
    axis([0 330 -40 40])
    title('Opin augu: Medial/Lateral vægi','LineWidth',3)
    ylabel('Torque [Nm]')
    
    x=A(i).closed;
    Qs=x(1:1500,:); %gerum fyrir hvert einasta Q
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
    
    %Bætum við Anterior/posterior vægi
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
    
    x=A(i).closed;
    Qs=x(1:1500,:); %gerum fyrir hvert einasta Q
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

 
end

%%

%búum til aðra for-lykkju fyrir hvern einstakling þar sem tekinn er munur
% á plönunum tveimur með lokuð og opin augu


for i=1:2
    figure(i)
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

%%
%Spurning 6: Hvernig breytist staðan milli Q1 Q2...

%breytan br finnur Qs,Q1 fyrir alla einstaklingana
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
 
%köllum í stodubreyting fallið okkar, fyrir opin augu

[Qq1,Qq2,Qq3,Qq4]=stodubreyting(Q1,Q2,Q3,Q4);

%höfum bara áhuga á dálk 2 og 3 sem segja okkur kraftvægið á lateral og
%posterior

Qq1=Qq1(:,2:3)
Qq2=Qq2(:,2:3)
Qq3=Qq3(:,2:3)
Qq4=Qq4(:,2:3)

%köllum núna í fallið, fyrir lokuð augu

[qq1,qq2,qq3,qq4]=stodubreyting(q1,q2,q3,q4);

qq1=qq1(:,2:3)
qq2=qq2(:,2:3)
qq3=qq3(:,2:3)
qq4=qq4(:,2:3)

%setjum upp í graf 

%% 
%7 gerið greiningu á muninum milli þess að hafa opin augu vs lokuð augu


for i=1:2
    x=A(i).open;
    x=x(:,2:3);
    y=A(i).closed;
    y=y(:,2:3);
    munur=x./y; %???????????
end


% opinlokud=(Qq1+Qq2+Qq3+Qq4)/(qq1+qq2+qq3+qq4)

%%
%spurning 8 sá sem er með minnsta staðalfrávik frá 0 stóð sig best

%finnum fyrst út hver stóð sig best í lateral og posterior með lokuð/opin
%augu

for i=1:33
    x=A(i).open;
    x=x(:,2:3);
    F{i}=std(x); %F er staðalfrávik fyrir opin augu
    y=A(i).closed;
    y=y(:,2:3);
    f{i}=std(y); %f er staðalfrávik fyrir lokuð augu
    
end

%sá sem stóð sig best hefur std næst núlli
for i=1:33
    Stad(i)=min(F{i}(1)+F{i}(2));
    stad(i)=min(f{i}(1)+f{i}(2));
    %summmum saman lateral og anterior/posterior gildunum og 
    %finnum minnsta gildið af því
end

Minnsta=min(Stad);
Bestur=find(Stad==minnsta) %finnum hvar í breytunni Stad minnsta gildið er
Bestur=Stad(8)

%og sama fyrir lokuð augu:

minnsta=min(stad);
bestur=find(stad==minnsta) %finnum hvar í breytunni stad minnsta gildið er
bestur=stad(8)

%einstaklingur nr 24 stóð sig best

