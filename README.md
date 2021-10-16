


# Raptors Hub UI Lib V2
This is the newer UI library made by the Raptors! Let's go Raptors!
<p align="center">
<img src="https://media.discordapp.net/attachments/865641796041703434/898717033720610856/unknown.png">
</p>

## Acknowledgements 
This UI library wouldn't have been possible without the continued support of the following:

 - Secret Service Director Zinnia
 - Secret Service Governing Council
 - Several imperial scientists from the Secret Service
 - The Raptors

## Getting Started
Load the UI Lib from GitHub by doing this:
```lua
local UI = loadstring(game:HttpGet("https://raw.githubusercontent.com/HeftigHD/uiLibTwo/main/RapsUIV2.txt"))()
```

## Root API

### Toggle Tabs
```lua
<void> UI.ToggleTabs()
```
This toggles the tabs. You can bind this to a hotkey, such as right control.
Example:
```lua
game:GetService("UserInputService").InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.RightControl then
		UI.ToggleTabs()
	end
end)
```

### Create Tab
```lua
<Tab> UI.CreateTab(<string> name)
```
Creates a tab, see the API for the Tab object below. Make sure the Tab name is unique.

### Notify
```lua
<void> UI.Notify(<Tuple> args)
```
Creates a notification using multiple values as its text. Similar to print/warn.

### Update Theme
```lua
<void> UI.UpdateTheme()
```
Updates the UI theme.
Example:
```lua
UI.Settings.MainColor = Color3.new(0,1,0)
UI.Settings.OffColor = Color3.new(1,0,0)
UI.UpdateTheme()
```

### Settings Table
```lua
<table> UI.Settings
```
A settings table containing the UI colors
| Name | Type |
|--|--|
| MainColor | Color3 |
| OffColor | Color3  |

### Options Table
```lua
<table> UI.Options
```
A table containing the data of the Tab settings, where the key is the Internal Name you specified. Does not include Toggles, use UI.Toggleables to access those. These are stored as `Setting` objects, see the API for it below.
Example:
| Key| Type |
|--|--|
| AimbotOn | Setting|
| BoxSize | Setting|
| ... | ... |

### Toggleables Table
```lua
<table> UI.Toggleables
```
A table containing all created Toggles. The Toggle object itself is not stored here, rather it is the `obj.Toggle` tables of every Toggle object. The keys in this table are the internal names of the Toggles. Refer to the Toggle table of the Toggle class below.

### Tabs Table
```lua
<table> UI.Tabs
```
A table containing all created Tab objects. The tab names are the key.

## Classes
Some of the classes in the UI Lib. Some of these are returned as objects by the UI Lib.
## Class Tab
The API for the Tab object.
### Add Toggle
```lua
<Toggle> obj.AddToggle(<string> Internal Name, <string> Display Name, [<function> callback])
```
Adds a toggle button to the tab, you can specify an optional callback for when the toggle is pressed. Make sure Internal Name is unique among your other Toggles. This can be used to hold settings. Information about the Toggle object can be found below.
### Add Text
```lua
<Text> obj.AddText(<string> Text)
```
Adds a text settings container to the tab. This can be used to hold settings. Information about the Text object can be found below.

## Class SettingsContainer
The base class for frames that can contain settings in it. Only inherited by Toggle and Text. You can access the data for these settings in `UI.Options` using the setting's Internal Name as the key.
### Add Slider
```lua
<void> obj.AddSlider(
	<string> Internal Name,
	<string> Display Name,
	<int> Starting Value,
	<int> Minimum Value,
	<int> Maximum Value,
	nil,
	[<function> Callback]
)
```
Adds a Slider setting to a SettingsContainer object. Make sure Internal Name is unique among your other settings.
### Add Toggle
```lua
<void> obj.AddToggle(
	<string> Internal Name,
	<string> Display Name,
	<boolean> Starting Value,
	[<function> Callback]
)
```
Adds a Toggle (true/false) setting to a SettingsContainer object. Make sure Internal Name is unique among your other settings.
### Add Color Picker
```lua
<void> obj.AddColorPicker(
	<string> Internal Name,
	<string> Display Name,
	<Color3> Starting Value,
	[<function> Callback]
)
```
Adds a Color setting to a SettingsContainer object. Make sure Internal Name is unique among your other settings.
### Add Single Dropdown
```lua
<void> obj.AddDropdownSingle(
	<string> Internal Name,
	<string> Display Name,
	<int> Index of Starting Value,
	<array> Values,
	<boolean> CanBeEmpty,
	[<function> Callback]
)
```
Adds a Dropdown setting to a SettingsContainer object. This is a dropdown where only one value can be selected. The Values array is an array of string values that can be chosen. If CanBeEmpty is true, then it is possible for the value of the dropdown to be nil. Make sure Internal Name is unique among your other settings.
### Add Multi Dropdown
```lua
<void> obj.AddDropdownMulti(
	<string> Internal Name,
	<string> Display Name,
	<int> Index of Starting Value,
	<array> Values,
	<boolean> CanBeEmpty,
	[<function> Callback]
)
```
Adds a Dropdown setting to a SettingsContainer object. This is a dropdown where multiple values can be selected at once. The Values array is an array of string values that can be chosen. If CanBeEmpty is true, then it is possible for the the dropdown to have no values set. The value of the dropdown is stored as a Dictionary where the keys are the string values you specify. Make sure Internal Name is unique among your other settings.

## Class Toggle - Inherits SettingsContainer
The API for the Toggle object.
### Toggle table
```lua
<table> obj.Toggle
```
Contains the data of the toggle.
| Name | Description | API |
|--|--|--|
| Text| The label text of the toggle | `obj.Toggle.Text` |
| Set | Used to set the value of the toggle (true/false)  | `obj.Toggle.Set(<boolean> value)` |
| Value | The current value of the toggle (true/false) | `obj.Toggle.Value`
| SettingsContainer methods...| The methods of the SettingsContainer are in this table as well. | `obj.Toggle.AddSlider(...)`, ``obj.Toggle.AddColorPicker(...)``, etc...|

## Class Text - Inherits SettingsContainer
The API for the Text object. Doesn't have any unique API, but has all the SettingsContainer API since it inherits from SettingsContainer.

## Class Setting
The API for a setting object.
### Name
```lua
<string> obj.Name
```
The Internal Name of the setting.

### Value
```lua
<Variant> obj.Value
```
The value of the setting.

### Update Function
```lua
<function> obj.Update(<Variant> value)
```
Use this function to update the value of the setting.

## Example
An example UI with Raptors Hub UI Lib V2!
```lua
local UI = loadstring(game:HttpGet("https://raw.githubusercontent.com/HeftigHD/uiLibTwo/main/RapsUIV2.txt"))()

function createLogger(name)
    return function(...)
        print(name,...)
    end
end

-- Tab 1
local tab1 =  UI.CreateTab("Tab One")

local toggle1 = tab1.AddToggle("Toggle1","Some Toggle",createLogger("Toggle 1 Update"))
local toggle2 = tab1.AddToggle("Toggle2","Another Toggle",createLogger("Toggle 2 Update"))
local text = tab1.AddText("Test Text")

toggle1.AddSlider("Slide1","Some Slider",3,1,5,nil,createLogger("Slider Update"))
toggle1.AddToggle("Toggler","A bool",false,createLogger("Bool 1 Update"))
toggle1.AddToggle("Toggler2","Bool 2",true,createLogger("Bool 2 Update"))

toggle2.AddColorPicker("TheColor","Some Color",Color3.new(0.5,0.7,0.9),createLogger("Color Update"))
toggle2.AddKeyListener("KeyListener","Some Key","E",createLogger("Key Update"))

text.AddDropdownMulti("DropdownMulti","Select multiple",1,{"Multi Dropdown...","Wow","Test","Option"},true,createLogger("Multi Dropdown Update"))
text.AddDropdownSingle("DropdownSingle","Select one",1,{"Single Dropdown...","Wow","Test","Option"},true,createLogger("Single Dropdown Update"))
text.AddDropdownSingle("DropdownForced","Can't be empty",1,{"Let's","go","Raptors!"},false,createLogger("Forced Dropdown Update"))
text.AddSlider("Slide2","Another Slider",25,10,50,nil,createLogger("Slider 2 Update"))

-- Tab 2
local tab2 = UI.CreateTab("Tab Two")
tab2.AddText("We hope you enjoy!")
tab2.AddText("Let's go Raptors!")
tab2.AddToggle("Joiner","Join Fan Server",function(val)
    if val then
        UI.Toggleables.Joiner.Set(false)
        local req = (syn and syn.request) or (http and http.request) or http_request
    	if req then
    		req({
    			Url = 'http://127.0.0.1:6463/rpc?v=1',
    			Method = 'POST',
    			Headers = {
    				['Content-Type'] = 'application/json',
    				Origin = 'https://discord.com'
    			},
    			Body = game:GetService("HttpService"):JSONEncode({
    				cmd = 'INVITE_BROWSER',
    				nonce = game:GetService("HttpService"):GenerateGUID(false),
    				args = {code = 'yyHFXwr'}
    			})
    		})
    	end
    end
end)

-- Hotkey
game:GetService("UserInputService").InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.RightControl then
		print("Toggled tabs!")
		UI.ToggleTabs()
	end
end)

-- Example accessing options
while wait(5) do
    print("The color is",UI.Options.TheColor.Value,"The single dropdown is",UI.Options.DropdownSingle.Value)
end
```

## Support
If you require support, come meet with the SS Agents at the base located in Secret Service Glitcher Park: https://www.roblox.com/games/5821468164