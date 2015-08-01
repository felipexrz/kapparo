# MVP Room Source #
```

//MVPController Script
//Kappa Ragnarok Online
//By Zachary E. Nowicki (znowicki93@gmail.com)
//March 2cd, 2011

//Last updated March 3rd, 2011

//RIGHTS TO DISTRIBUTE AND MODIFY WITH PERMISSION AND CREDIT. THANK YOU. 

-	script	MVPController	-1,{

//WHISPER CONTROL
OnWhisperGlobal:

//ANTI-NUB\\
if(getgmlevel() != 99) { end; }

//MVP LIST
//---------
setarray .mvplist[0], 0, 1038, 1039, 1046, 1059, 1086, 1087, 1096, 1112, 1115, 1120, 1147, 1150, 1157, 1159, 1190, 1251, 1252, 1272, 1312, 1373, 1388, 1389, 1418, 1492, 1511, 1582, 1583, 1623, 1630, 1658, 1685, 1688, 1708, 1719, 1734, 1751, 1768, 1779, 1785, 1832, 1871, 1873, 1885;
//---------

//Commands:
if(@whispervar0$=="start") { //First MVP Startup

	set $@run,1; 
	announce "Public MVP Room Started! Click the MVP Room Warper to join the fun!",0;
	set .spawn,getelementofarray(.mvplist,rand(45));
	//announce .spawn,bc_map;
	monster "2008rwc_08",0,0,strmobinfo(1,.spawn),.spawn,1,"MVPController::OnMVPKill";
	mapannounce "2008rwc_08","A " + strmobinfo(1,.spawn) + " has been spawned! Level: " + strmobinfo(3,.spawn) + " HP: " + strmobinfo(4,.spawn),0; 
	doevent "Public MVP Room#mvpwarp::OnPMVPRoomOpen";
	end;

} 

if(@whispervar0$=="stop")  { set $@run,0; announce "Public MVP Room is now Closed.",0; doevent "Public MVP Room#mvpwarp::OnPMVPRoomClose"; mapwarp "2008rwc_08","prontera",155,187; end; }  //.. and you're a moron if you don't get this one
if(@whispervar0$=="clear") { killmonster "2008rwc_08","MVPController::OnMVPKill"; mapannounce "2008rwc_08","MVP Room cleared by GM",0; } //Forcekill

OnMVPKill:
if($@run==1) {
	//attachrid(2000001);
	mapannounce "2008rwc_08",strcharinfo(0) + " has killed the MVP! Good Work!",0,0xFF6600;
	atcommand "@effect 500";
	atcommand "@effect 410";
	atcommand "@effect 328";
	initnpctimer;
	end;
}

OnTimer3500:
if($@run==1) {
attachrid(2000001);
mapannounce "2008rwc_08","Spawning next MVP...",0,0x66FF22;
end;
}


OnTimer10000:
if($@run==1){
attachrid(2000001);
set .spawn,getelementofarray(.mvplist,rand(45));
monster "2008rwc_08",0,0,strmobinfo(1,.spawn),.spawn,1,"MVPController::OnMVPKill";
mapannounce "2008rwc_08","A " + strmobinfo(1,.spawn) + " has been spawned! Level: " + strmobinfo(3,.spawn) + " HP: " + strmobinfo(4,.spawn),0,0xFF0000;
stopnpctimer;
}

}

2008rwc_08,50,52,4	script	MVP Master#mvpmas	803,{

if(@notalk != 1) {
	mes "[MVP Master]";
	mes "Welcome to the Public MVP Room!";
	mes "Here, random MVPs and mobs will spawn";
	mes "regularly for you to combat!";
	next;

	mes "[MVP Master]";
	mes "Click me if you";
	mes "would like to spawn a";
	mes "specific MVP,";
	mes "or need a Heal.";
	set @notalk,1;
	close;
}

mes "[MVP Master]";
mes "Welcome, select an option.";
menu "Heal me, please!",L_heal,"Spawn a certain MVP",L_spawn;

L_heal:
percentheal 100,100;
close;

L_spawn:
next;
mes "[MVP Master]";
mes "I apologize,";
mes "This feature is not yet";
mes "available. You will be";
mes "notified when it is";
mes "implemented.";
close;

}


//MVP Room Warper
prontera,149,193,6	script	Public MVP Room#mvpwarp	429,{

//Room closed - idle chitchat.
if($@op==0) {

	mes "^FF0000[MVP Room Warper]^000000";
	mes "Hi!";
	mes "Click on me when";
	mes "the Public MVP Room";
	mes "is open, and I'll";
	mes "warp you there!";
	next;
	mes "^FF0000[MVP Room Warper]^000000";
	mes "For more information";
	mes "regarding this room,";
	mes "please contact an";
	mes "administrator.";
	mes " ";
	mes "Thanks!";
	close;

}

if($@op==1) {

	if(Class <= 20) {
		mes "^FF0000[MVP Room Warper]^000000";
		mes "Sorry, only rebirth classes";
		mes "and above can join!";
		next;
		mes "^FF0000[MVP Room Warper]^000000";
		mes "Come back when you've";
		mes "been reborn.";
		close;
	}

	//Else, player is not a nub....
	percentheal 100,100;  //I thought I would be nice and heal them before they joined. Aren't I sweet?
	warp "2008rwc_08",50,59; //warpwarpwarp
	end;
}

OnPMVPRoomOpen:
	
	set $@op,1;
	waitingroom "Public MVP Room Open!",1;
	end;

OnPMVPRoomClose:

	set $@op,0;
	delwaitingroom;
	end;

}

```