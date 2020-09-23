<div align="center">

## System Tray Class \(Very Easy To Use\)


</div>

### Description

This class I developed makes it very easy to put/delete/modify icons in the system tray. You can also get mouse notifications from the system tray. Once again, NO MFC!
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Mike A\. Leonetti](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/mike-a-leonetti.md)
**Level**          |Beginner
**User Rating**    |5.0 (20 globes from 4 users)
**Compatibility**  |C, C\+\+ \(general\)
**Category**       |[Classes/ Frameworks/ OOP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/classes-frameworks-oop__3-31.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/mike-a-leonetti-system-tray-class-very-easy-to-use__3-538/archive/master.zip)

### API Declarations

```
#include <Shellapi.h>
```


### Source Code

```
//Class Code (Copy And Paste it Somewhere)//
#define WM_TRAYNOTIFY 0xA44C
#include <Shellapi.h>
class clsSysTray
	{
		public:
			clsSysTray();
			BOOL SetIcon(HICON hNewIcon);
			HICON GetIcon();
			BOOL SetTipText(char *lpstrNewTipText);
			char *GetTipText();
			BOOL AddIcon();
			BOOL RemoveIcon();
			HWND hWnd;
			UINT uID;
		protected:
			NOTIFYICONDATA NotifyIconData;
			bool bInTray;
	};
clsSysTray::clsSysTray()
	{
		bInTray = false;
		NotifyIconData.cbSize = sizeof(NotifyIconData);
		NotifyIconData.uID = uID = 2;
		NotifyIconData.uFlags = NIF_ICON | NIF_TIP | NIF_MESSAGE;
		NotifyIconData.uCallbackMessage = WM_TRAYNOTIFY;
		NotifyIconData.hIcon = LoadIcon(NULL, IDI_APPLICATION);
		NotifyIconData.szTip[0] = '\0';
		NotifyIconData.hWnd = NULL;
	}
HICON clsSysTray::GetIcon()
	{
		return(NotifyIconData.hIcon);
	}
BOOL clsSysTray::SetIcon(HICON hNewIcon)
	{
		NotifyIconData.hIcon = hNewIcon;
		if(bInTray)
			{
				BOOL iRetVal;
				iRetVal = Shell_NotifyIcon(NIM_MODIFY, &NotifyIconData);
				if(iRetVal)
					{
						bInTray = true;
					}
				return(iRetVal);
			}
		else
			return(1);
	}
char *clsSysTray::GetTipText()
	{
		return(NotifyIconData.szTip);
	}
BOOL clsSysTray::SetTipText(char *lpstrNewTipText)
	{
		strcpy(NotifyIconData.szTip, lpstrNewTipText);
		if(bInTray)
			{
				BOOL iRetVal;
				iRetVal = Shell_NotifyIcon(NIM_MODIFY, &NotifyIconData);
				if(iRetVal)
					{
						bInTray = true;
					}
				return(iRetVal);
			}
		else
			return(1);
	}
BOOL clsSysTray::AddIcon()
	{
		BOOL iRetVal;
		NotifyIconData.hWnd = hWnd;
		NotifyIconData.uID = hWnd;
		iRetVal = Shell_NotifyIcon(NIM_ADD, &NotifyIconData);
		if(iRetVal)
			{
				bInTray = true;
			}
		return(iRetVal);
	}
BOOL clsSysTray::RemoveIcon()
	{
		BOOL iRetVal;
		iRetVal = Shell_NotifyIcon(NIM_DELETE, &NotifyIconData);
		if(iRetVal)
			{
				bInTray = false;
			}
		return(iRetVal);
	}
//End Class Code//
//Example Of Use///
.
.
.
clsSysTray SystemTrayEx;
SystemTrayEx.hWnd = hWnd;
SystemTrayEx.SetTipText("HI");
SystemTrayEx.AddIcon();
SystemTrayEx.SetIcon(hApp_Icon_Main);
.
.
.
//End Example//
///Example Of Callback (Notification) Messages//
.
.
.
	switch(Msg)
		{
		case WM_TRAYNOTIFY:
 	{
 		switch(LOWORD(lParam))
 			{
 	case WM_LBUTTONDBLCLK:
						case WM_LBUTTONDOWN:
						case WM_LBUTTONUP:
						case WM_RBUTTONDBLCLK:
						case WM_RBUTTONDOWN:
						case WM_RBUTTONUP:
 			}
		return(0);
		} break;
.
.
.
//End Callback Example//
```

