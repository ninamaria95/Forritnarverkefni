# Forritnarverkefni
Forritunarverkefni 

slodi=uigetdir('Veldu moppu')

for i=1:33
    skra=strcat('SUB',num2str(i),'_closed','.xlsx')
    s=strcat(slodi,'/',skra)
    A=xlsread(s)
end

slodi=uigetdir('Veldu moppu')

for i=1:33
    skra=strcat('SUB',num2str(i),'_open','.xlsx')
    s=strcat(slodi,'/',skra)
    A=xlsread(s)
end

slodi=uigetdir('Veldu moppu')

skra=strcat('Stimuli','.xlsx')
s=strcat(slodi,'/',skra)
A=xlsread(s)
