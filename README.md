# Forritnarverkefni
Forritunarverkefni 

slodi=uigetdir('Veldu moppu')

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

Tidni=B(:,2)

for i=1:length(Tidni)
    if Tidni(i)==20
        Tidni(i)=0;
    elseif Tidni(i)==59
        Tidni(i)=1;
    end  
end
B(:,2)=Tidni;

%Í lið 2 breyttum við söfnunartíðninni 50 Hz í sekúndur og var það 1/50 sek sem er u.þ.b. 0.02 sek
%finnum núna fjölda stimula/örvana
x=find(B(:,2)==1);   %hérna er staðsetningin á örvunum
stnum=length(x);

%%Hérna er fallið okkar fyrir lið heildarfjölda stimuli og meðaltíma

function [stnum y]=stimulifall(B)
%búum til fall sem telur fjölda stimula og reiknar út meðaltímalengd þeirra
x=find(B(:,2)==1);
stnum=length(x);
y=mean(B(x));

fprintf('Heildarfjöldi stimuli er %0.0f \n',stnum)
fprintf('Meðaltími hvers stimulus er %0.2f sekúndur \n',y)
end

%% kallað í fallið

hfjoldi=stimulifall(B);

%gerum for lykkju fyrir alla 33 einstaklingana

for i=1:2
    figure(i)
  
    %núna setjum við inn stimuli hlutann
    subplot(3,2,1)
    plot(B(1:1500,1),B(1:1500,2),'m',B(1501:5250,1),B(1501:5250,2),'r',...
        B(5251:9000,1),B(5251:9000,2),'g',B(9001:12750,1),B(9001:12750,2),'b',...
        B(12751:16500,1),B(12751:16500,2),'k')
    hold
    set(gca,'XTickLabel',[0:50:330])
    set(gca,'YTickLabel',{' '})
    title('Stimuli','FontSize',7.3)
    
    %Stimuli mynd nr 2
    subplot(3,2,2)
    plot(B(1:1500,1),B(1:1500,2),'m',B(1501:5250,1),B(1501:5250,2),'r',...
        B(5251:9000,1),B(5251:9000,2),'g',B(9001:12750,1),B(9001:12750,2),'b',...
        B(12751:16500,1),B(12751:16500,2),'k')
    hold
    set(gca,'XTickLabel',[0:50:330])
    set(gca,'YTickLabel',{' '})
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
   
    
end




??? spurja hvor við eigum að gera fall fyrir plot
