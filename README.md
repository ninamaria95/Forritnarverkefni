# Forritnarverkefni
Forritunarverkefni 

close all 
clear all
clc
slodi=uigetdir('Veldu moppu')

for i=1:33;
    skra=strcat('SUB',num2str(i),'_closed','.xlsx');
    s=strcat(slodi,'/',skra);
    A=xlsread(s);
end

for i=1:33;
    skra=strcat('SUB',num2str(i),'_open','.xlsx');
    s=strcat(slodi,'/',skra);
    A=xlsread(s);
end

skra=strcat('Stimuli','.xlsx');
s=strcat(slodi,'/',skra);
A=xlsread(s);
