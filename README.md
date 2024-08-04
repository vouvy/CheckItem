<table>
<tr>
<td>
This script is a basic example of the Lua programming language. It was created for personal use, but feel free to use or study it
</td>
</tr>
</table>

---

## Features
- CheckItem Function: Determines if a specific item is present in the player's backpack or character.
- CheckLevel Function: Retrieves the level of an item, if it exists.
- CreateFile Function: Creates a text file named after the player with the content "Yummytool."
- CheckConditions Function: Validates if the player meets certain criteria, including having specific item levels and a minimum amount of in-game currency ("Beli").
- CheckConditionsWithKatana Function: Extends CheckConditions by also requiring a specific level for the "Cursed Dual Katana" item.
- Main Loop: Continuously checks if the player has the required items and conditions every 5 minutes (300 seconds). If all conditions are met, it creates a file; otherwise, it prints a message indicating insufficient requirements.

## Code Example:
```lua
local function CheckItem(ItemName)
  return game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(ItemName) or game:GetService("Players").LocalPlayer.Character:FindFirstChild(ItemName) or false
end

local function CheckLevel(Item)
  if Item and Item:FindFirstChild("Level") then
      return Item.Level.Value
  end
  return 0
end

local function CreateFile()
  writefile(game.Players.LocalPlayer.Name..".txt", "Yummytool")
  print("Completed")
end

local function CheckConditions(GodHumanLevel, FruitLevels, BeliValue)
  local requiredLevels = {
      GodHuman = 1,
      Buddha = 1,
      Dark = 1,
      Mammoth = 1,
      Leopard = 1,
      Dough = 1,
      Kitsune = 1,
      Rex = 1,
      Sand = 1,
      Ice = 1
  }

  if GodHumanLevel < requiredLevels.GodHuman or BeliValue <= 5000000 then
      return false
  end

  for fruit, level in pairs(FruitLevels) do
      if level < requiredLevels[fruit] then
          return false
      end
  end
  return true
end

local function CheckConditionsWithKatana(GodHumanLevel, FruitLevels, CDKatanaLevel, BeliValue)
  return CheckConditions(GodHumanLevel, FruitLevels, BeliValue) and CDKatanaLevel >= 1
end

while true do
  local GodHuman = CheckItem("Godhuman")
  local CDKatana = CheckItem("Cursed Dual Katana")
  local Beli = game:GetService("Players").LocalPlayer:FindFirstChild("Data"):FindFirstChild("Beli")

  local Fruits = {
    Buddha = CheckItem("Buddha-Buddha"),
    Dark = CheckItem("Dark-Dark"),
    Mammoth = CheckItem("Mammoth-Mammoth"),
    Leopard = CheckItem("Leopard-Leopard"),
    Dough = CheckItem("Dough-Dough"),
    Kitsune = CheckItem("Kitsune-Kitsune"),
    Rex = CheckItem("T-Rex-T-Rex"),
    Sand = CheckItem("Sand-Sand"),
    Ice = CheckItem("Ice-Ice")
  }

  if GodHuman and Beli then
    local GodHumanLevel = CheckLevel(GodHuman)
    local CDKatanaLevel = CDKatana and CheckLevel(CDKatana) or 0
    local BeliValue = Beli.Value
    local FruitLevels = {}

    for fruitName, fruit in pairs(Fruits) do
      if fruit then
        FruitLevels[fruitName] = CheckLevel(fruit)
      end
    end

    if (CDKatana and CheckConditionsWithKatana(GodHumanLevel, FruitLevels, CDKatanaLevel, BeliValue)) or 
       (not CDKatana and CheckConditions(GodHumanLevel, FruitLevels, BeliValue)) then
      CreateFile()
    else
      print("Not Enough")
    end
  else
    print("Not Enough")
  end

  wait(300)
end
```
