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
A(1).stimuli=xlsread(s);


