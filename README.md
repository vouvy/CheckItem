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
