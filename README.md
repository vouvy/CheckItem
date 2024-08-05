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
-- Function to check if an item exists in the player's Backpack or Character
local function CheckItem(ItemName)
  -- Return true if the item is found in either Backpack or Character, otherwise return false
  return game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(ItemName) or 
         game:GetService("Players").LocalPlayer.Character:FindFirstChild(ItemName) or 
         false
end

-- Function to check the level of an item if it exists
local function CheckLevel(Item)
  -- Return the value of the Level if the item has a Level property
  if Item and Item:FindFirstChild("Level") then
      return Item.Level.Value
  end
  -- Return 0 if the item does not have a Level property
  return 0
end

-- Function to create a file with the player's name and a specific content
local function CreateFile()
  -- Write "Yummytool" to a text file named after the player's name
  writefile(game.Players.LocalPlayer.Name..".txt", "Yummytool")
  -- Print a completion message
  print("Completed")
end

-- Function to check if conditions are met based on levels and value
local function CheckConditions(GodHumanLevel, FruitLevels, BeliValue)
  -- Define the required levels for each fruit
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

  -- Check if GodHuman level is sufficient and Beli value is greater than 5,000,000
  if GodHumanLevel < requiredLevels.GodHuman or BeliValue <= 5000000 then
      return false
  end

  -- Check if all fruit levels meet or exceed the required levels
  for fruit, level in pairs(FruitLevels) do
      if level < requiredLevels[fruit] then
          return false
      end
  end
  -- Return true if all conditions are met
  return true
end

-- Function to check conditions with an additional requirement for CDKatana level
local function CheckConditionsWithKatana(GodHumanLevel, FruitLevels, CDKatanaLevel, BeliValue)
  -- Return true if conditions are met and CDKatana level is at least 1
  return CheckConditions(GodHumanLevel, FruitLevels, BeliValue) and CDKatanaLevel >= 1
end

-- Infinite loop to periodically check conditions and create a file if conditions are met
while true do
  -- Check if the player has specific items
  local GodHuman = CheckItem("Godhuman")
  local CDKatana = CheckItem("Cursed Dual Katana")
  local Beli = game:GetService("Players").LocalPlayer:FindFirstChild("Data"):FindFirstChild("Beli")

  -- Check if the player has each of the specific fruits
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

  -- If the player has GodHuman and Beli items
  if GodHuman and Beli then
    -- Get the level of GodHuman and CDKatana (if it exists), and the value of Beli
    local GodHumanLevel = CheckLevel(GodHuman)
    local CDKatanaLevel = CDKatana and CheckLevel(CDKatana) or 0
    local BeliValue = Beli.Value
    local FruitLevels = {}

    -- Get the levels of all the fruits
    for fruitName, fruit in pairs(Fruits) do
      if fruit then
        FruitLevels[fruitName] = CheckLevel(fruit)
      end
    end

    -- Check conditions with or without CDKatana and create a file if conditions are met
    if (CDKatana and CheckConditionsWithKatana(GodHumanLevel, FruitLevels, CDKatanaLevel, BeliValue)) or 
       (not CDKatana and CheckConditions(GodHumanLevel, FruitLevels, BeliValue)) then
      CreateFile()
    else
      -- Print "Not Enough" if conditions are not met
      print("Not Enough")
    end
  else
    -- Print "Not Enough" if GodHuman or Beli are missing
    print("Not Enough")
  end

  -- Wait for 300 seconds before repeating the loop
  wait(300)
end
```
