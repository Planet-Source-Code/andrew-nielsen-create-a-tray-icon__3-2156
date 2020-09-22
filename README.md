<div align="center">

## Create a Tray Icon


</div>

### Description

Teach users to create a Tray Icon and change the Tip Text and Icon when ever they want with out re-creating the tray icon.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Andrew Nielsen](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/andrew-nielsen.md)
**Level**          |Intermediate
**User Rating**    |3.0 (9 globes from 3 users)
**Compatibility**  |Microsoft Visual C\+\+
**Category**       |[Miscellaneous](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/miscellaneous__3-1.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/andrew-nielsen-create-a-tray-icon__3-2156/archive/master.zip)





### Source Code

I have supplied you with some sample code, I will just explain what I did and why I did it so you can understand what is going on :-)<br>
I swipped this code from my CD Player, so for those of you who might recognize this, that's where it came from.<br>
first let's create the Icon Variable<br>
NOTIFYICONDATA	nid;<br>
void CreateTrayIcon(HWND hWnd, HINSTANCE hInst)<br>
{<br>
First we set the size of the TrayIcon variable:<br>
	nid.cbSize = sizeof(nid);<br>
I initialized the main Icon here<br>
	TrayIcon =	(HICON)LoadImage(hInst, MAKEINTRESOURCE(IDI_TRAY),	IMAGE_ICON, 16, 16, LR_DEFAULTCOLOR);<br>
Now I set the main Icon into the Tray Icon, you will see soon how to change this with out resetting the Icon.<br>
	nid.hIcon = TrayIcon;<br>
Here I gave the Icon something to 'report to' all the user inputs...<br>
	nid.hWnd = hWnd;<br>
Mouse over tip text, you will see how to change this also.<br>
	strcpy(nid.szTip, "CD Player");<br>
Now I set the Callback Message, I will explain this later in the code.<br>
	nid.uCallbackMessage = WM_TRAYICON;<br>
Here I told the Tray Icon what I wanted it to do, I have it making the Icon and the Tip, aswell as having the Tray notice it has been assigned a Callback Message<br>
	nid.uFlags = NIF_ICON | NIF_MESSAGE | NIF_TIP;<br>
Give the Tray Icon the main Windows Instance<br>
	nid.uID = (UINT)hInst;<br>
This just created the Icon:<br>
	Shell_NotifyIcon(NIM_ADD, &nid);<br>
}<br>
void ShowTrayIcon(void)<br>
{<br>
You might be thinking: 'Why did I add the Icon again?', but when I hide the icon I deleted it from the tray...<br>
	Shell_NotifyIcon(NIM_ADD, &nid);<br>
}<br>
void HideTrayIcon(void)<br>
{<br>
...as you can see here.<br>
	Shell_NotifyIcon(NIM_DELETE, &nid);<br>
}<br>
void SetTrayIconIcon(HICON icon)<br>
{<br>
To change the Icon, just make icon equal to the icon in which you wish it to be<br>
	nid.hIcon = icon;<br>
	Shell_NotifyIcon(NIM_MODIFY, &nid);<br>
}<br>
void SetTrayIconCaption(char *msg)<br>
{<br>
Same here, just set msg to the message you want to be displayed on mouse over of the Tray Icon<br>
	strcpy(nid.szTip, msg);<br>
	Shell_NotifyIcon(NIM_MODIFY, &nid);<br>
}<br>
To clean up the icon when the program is shutting down...<br>
void KillTrayIcon(void)<br>
{<br>
...first we delete the Icon from the tray...<br>
	Shell_NotifyIcon(NIM_DELETE, &nid);<br>
...then we destroy the main Icon...<br>
	DestroyIcon(TrayIcon);<br>
...and reset the Tray Icon variable back to nothing.<br>
	memset(&nid, 0, sizeof(nid));<br>
}<br>
now we can 'handle' all input from the Icon and show a menu when it's right or left clicked on:<br>
this is where the Callback Message comes into play... whenever the icon is clicked on, whether it be left or right click, WM_TRAYICON is sent to the window procedure for processing<br>
case WM_TRAYICON:<br>
{<br>
to catch any clicks you make a switch/case loop using the LPARAM parameter of the windows procedure<br>
	switch(lParam)<br>
	{<br>
then you can have it check to see if it was a right button or a left button that was clicked.<br>
		case WM_RBUTTONDOWN:<br>
here I just have it getting the cursor position and displaying a menu where ever the right mouse button was pressed<br>
			POINT pp;<br>
			GetCursorPos(&pp);<br>
this just keeps in a respectable place on the screen...<br>
			if (pp.x < (long) (GetSystemMetrics(SM_CXSCREEN) - 100))<br>
						pp.x = (long) (GetSystemMetrics(SM_CXSCREEN) - 100);<br>
			if (pp.y < (long) (GetSystemMetrics(SM_CYSCREEN) - 100))<br>
						pp.y = (long) (GetSystemMetrics(SM_CYSCREEN) - 100);<br>
									SetForegroundWindow(hWnd);<br>
									TrackPopupMenu(hPopup, TPM_RIGHTBUTTON, pp.x, pp.y, 0, hWnd, NULL);<br>
			break;<br>
for the left button I have it showing the window if I had it minimized:<br>
		case WM_LBUTTONDOWN:<br>
			if(Minimized)<br>
			{<br>
			ShowWindow(hWnd, SW_SHOW);<br>
			}<br>
	}<br>
	break;<br>
}<br>
to process any menu commands, just process that the way you would with a normal window menu.<br>
That pretty much covers it! if you hve any further questions about <b>ANY</b> of my tutorials <a href="mailto:dragoon_xxx@hotmail.com">E-Mail me</a>, and I will answer your questions as best as I can.

