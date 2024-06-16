local operators = {}
local currentAttacker = "Zofia"
local currentDefender = "Solis"
 
local attackerMappings = {
    Ace = "Amaru", Amaru = "Ash", Ash = "Blackbeard", Blackbeard = "Buck", Buck = "Capitao", Capitao = "Dokkaebi",
    Dokkaebi = "Finka", Finka = "Deimos", Flores = "Fuze", Fuze = "Glaz", Glaz = "Gridlock", Gridlock = "Grim", Grim = "Hibana",
    Hibana = "Iana", Iana = "IQ", IQ = "Ram", Jackal = "Kali", Kali = "Lion", Lion = "Maverick", Maverick = "Nokk", Nokk = "Nomad",
    Nomad = "Osa", Osa = "Sens", Sens = "Brava", Sledge = "Thatcher", Thatcher = "Thermite", Thermite = "Twitch", Twitch = "Ying",
    Ying = "Zero", Zero = "Zofia", Zofia = "Ace", Deimos = "Flores", Ram = "Jackal", Brava = "Sledge",
}
 
local defenderMappings = {
    Alibi = "Aruni", Aruni = "Azami", Azami = "Bandit", Bandit = "Castle", Castle = "Caveira", Caveira = "Clash",
    Clash = "Doc", Doc = "Echo", Echo = "Ela", Ela = "Frost", Frost = "Goyo", Goyo = "Jager", Jager = "Kaid", Kaid = "Kapkan",
    Kapkan = "Lesion", Lesion = "Maestro", Maestro = "Melusi", Melusi = "Mira", Mira = "Mozzie", Mozzie = "Mute", Mute = "Oryx",
    Oryx = "Pulse", Pulse = "Rook", Rook = "Smoke", Smoke = "Tachanka", Tachanka = "Thorn", Thorn = "Thunderbird", Thunderbird = "Valkyrie",
    Valkyrie = "Vigil", Vigil = "Wamai", Wamai = "Warden", Warden = "Tubarao", Tubarao = "Fenrir", Fenrir = "Solis", Solis = "Alibi",
}
 
local selectedOperator = currentAttacker
 
local operatorStats = {
Ace = { -1, 22, 150, -1, 16, 500, -1, 19, 500, -1, 20, 800, -1, 14, 200 },
Amaru = { 0, 16, 150, -1, 14, 400, -1, 14, 700, 1, 20, 300, -1, 13, 150, -2, 16, 400, -2, 14, 550, -2, 16, 300, -2, 20, 250, -2, 15, 100 },
Ash = { -1, 23, 150, -1, 20, 600, -2, 22, 1300, 1, 18, 2300, 1, 22, 3200 },
Blackbeard = { 0, 10, 150, 0, 9, 250, 1, 7, 650, 1, 9, 1050 },
Buck = { 0, 17, 150, 0, 14, 650, 0, 15, 1050, 1, 19, 500 },
Capitao = { 0, 15, 150, 0, 12, 400, -1, 13, 1000, -1, 17, 1050 },
Dokkaebi = { 1, 0, 150, 2, 36, 350, -4, 6, 500, -3, 33, 500 },
Finka = { 0, 22, 80, -1, 17, 700, -1, 22, 700, -1, 22, 1500, -2, 19, 6100 },
Flores = { 0, 16, 80, -1, 15, 350, 0, 13, 300, -1, 14, 200, 0, 14, 1400 },
Fuze = { 0, 19, 80, -1, 17, 700, -1, 20, 700, -1, 22, 1500, -2, 22, 6100 },
Glaz = { 0, 26, 60, 1, 24, 1600 },
Gridlock = { 0, 17, 150, -1, 15, 600, -1, 17, 1300 },
Grim = { 0, 17, 110, -1, 13, 500, -1, 13, 500, 0, 14, 850, -1, 17, 800 },
Hibana = { -1, 24, 40, -1, 17, 600, -2, 17, 300, -3, 18, 550 },
Iana = { 0, 17, 60, -1, 13, 1100, -1, 17, 550, -1, 16, 300 },
IQ = { 0, 17, 110, -1, 13, 500, -1, 13, 500, 0, 14, 850, -1, 15, 800 },
Jackal = { 0, 22, 70, 0, 19, 500, -1, 18, 600, -1, 24, 700 },
Kali = { 0, 16, 70, 1, 13, 200, -2, 13, 300, 4, 13, 1100 },
Lion = { 0, 17, 60, 0, 14, 300, 0, 12, 1700, 0, 17, 2300 },
Maverick = { 0, 23, 40, -1, 15, 200, -1, 16, 800, -1, 18, 1360 },
Nokk = { 0, 23, 40, 1, 15, 200, 0, 15, 800, 1, 15, 1100 },
Nomad = { 0, 20, 70, -1, 10, 1400, -1, 12, 1700, -1, 12, 750 },
Osa = { 0, 12, 80, 1, 11, 600, 1, 12, 300, 1, 12, 1800 },
Sens = { 1, 18, 40, 1, 13, 400, 0, 14, 1600, -1, 18, 300, -1, 17, 1800 },
Sledge = { 0, 18, 60, 1, 12, 450, 1, 10, 300, 1, 12, 2000 },
Thatcher = { 0, 14, 80, -1, 13, 350, 0, 13, 300, -1, 12, 200, 0, 12, 1400 },
Thermite = { 0, 11, 80, 1, 12, 600, 2, 11, 300, 1, 10, 1800 },
Twitch = { 0, 26, 80, -1, 22, 600, 0, 24, 200, -1, 24, 1000 },
Ying = { 0, 16, 60, 0, 12, 300, 0, 13, 450, 0, 14, 1800, 1, 15, 4800 },
Zero = { 0, 22, 40, -1, 16, 200, -1, 19, 700, -1, 19, 350, 0, 19, 650 },
Zofia = { 0, 22, 90, 1, 16, 600, 1, 16, 500, 0, 19, 1500 },
Alibi = { 0, 24, 60, 1, 17, 600, 1, 19, 200, 1, 20, 300, 2, 20, 550, 1, 20, 350 },
Aruni = { 0, 30, 10, 1, 17, 1000 },
Azami = { 0, 30, 10, -1, 16, 650, -1, 17, 800, 0, 18, 400, 0, 19, 500 },
Bandit = { 0, 18, 70, 1, 14, 800, 1, 16, 300, 0, 17, 650 },
Castle = { 0, 22, 30, -1, 10, 250, 0, 10, 200, 0, 10, 400, -1, 9, 700, 0, 10, 1100 },
Caveira = { 0, 8, 1000, -1, 8, 900, 0, 9, 600, -1, 8, 800 },
Clash = { 0, 10, 1000, -1, 10, 900, -1, 11, 600, -1, 10, 800 },
Doc = { 0, 23, 30, -1, 14, 600, 0, 15, 300, -1, 16, 500, -1, 18, 900 },
Echo = { 0, 22, 30, -1, 15, 600, 0, 16, 300, -1, 17, 1400 },
Ela = { 0, 26, 100, 3, 18, 500, 3, 27, 300, 3, 26, 900, 4, 26, 600 },
Frost = { 0, 15, 30, 0, 9, 600, 0, 9, 1100, 1, 10, 500, 0, 11, 1300 },
Goyo = { 0, 24, 60, 0, 19, 250, 0, 24, 150, 0, 24, 300, 1, 24, 600 },
Jager = { 0, 32, 60, -1, 17, 300, -1, 19, 1000, 0, 20, 700, -1, 20, 200 },
Kaid = { 0, 26, 60, 1, 13, 300, 1, 13, 800, 0, 15, 1650 },
Kapkan = { 0, 22, 10, 0, 14, 350, -1, 15, 300, -1, 15, 800, 0, 16, 400, -1, 17, 500 },
Lesion = { 0, 22, 50, 0, 17, 500, -1, 19, 300, 0, 20, 300, 0, 20, 100, 1, 20, 900 },
Maestro = { 0, 39, 20, 0, 18, 500, 0, 19, 500, 0, 20, 500, -1, 21, 600, 0, 21, 600, 0, 22, 2600 },
Melusi = { 0, 28, 40, 0, 14, 600, -1, 17, 1000, -1, 18, 700 },
Mira = { 0, 27, 60, 1, 20, 250, 2, 22, 300, 2, 23, 700 },
Mozzie = { 1, 29, 50, 2, 13, 300, 2, 14, 300, 1, 14, 400, 1, 16, 400, 1, 15, 700 },
Mute = { 0, 38, 60, 0, 26, 500, 0, 27, 200 },
Oryx = { 0, 23, 50, 0, 17, 500, 0, 20, 300, 1, 20, 300, 0, 21, 100, 1, 21, 600, 4, 21, 300 },
Pulse = { 0, 22, 30, 0, 10, 250, -1, 10, 200, 0, 10, 400, -2, 9, 700, 0, 10, 1100 },
Rook = { 0, 27, 20, -1, 15, 700, 0, 18, 200, -1, 18, 600, 0, 15, 300, -2, 15, 500 },
Smoke = { 0, 38, 60, 0, 26, 500, 0, 27, 200 },
Tachanka = { 0, 20, 20, 1, 6, 300, -2, 6, 200, 1, 7, 400, 1, 7, 500, 2, 2, 1200, 3, 6, 1200, 2, 2, 1200, 2, 2, 600, 2, 2, 1700 },
Thorn = { 0, 26, 50, 0, 15, 500, 1, 16, 500, 1, 18, 500, 0, 19, 300 },
Thunderbird = { 0, 32, 30, -1, 18, 250, 0, 18, 250, -1, 20, 500, -1, 21, 500, -1, 20, 500, 0, 21, 700 },
Valkyrie = { 0, 29, 20, 0, 14, 500, -1, 16, 500, -1, 16, 500, 1, 17, 500, 1, 18, 300 },
Vigil = { 0, 27, 30, -1, 15, 300, -1, 17, 500, -1, 18, 700, -1, 17, 500, -1, 17, 500 },
Wamai = { 0, 24, 40, 0, 15, 300, -1, 13, 400, 1, 15, 1400, 2, 15, 500, 2, 16, 200 },
Warden = { 0, 25, 20, 0, 14, 500, -1, 14, 500, -1, 14, 500, 1, 14, 500, 1, 15, 300 },
 
Tubarao =  { 0, 29, 20, 0, 14, 500, -1, 16, 500, -1, 16, 500, 1, 17, 500, 1, 18, 300 },
Fenrir = { 0, 18, 70, 1, 14, 800, 1, 16, 300, 0, 17, 650 },
Solis = { 0, 23, 30, -1, 14, 600, 0, 15, 300, -1, 16, 500, -1, 18, 900 },
Deimos = { 0, 20, 70, -1, 10, 1400, -1, 12, 1700, -1, 12, 750 },
Ram = { -1, 23, 150, -1, 20, 600, -1, 22, 1300 },
Brava = { 0, 15, 150, 0, 12, 400, -1, 13, 1000, -1, 17, 1050 },
    
}
 
function switchAttacker()
    currentAttacker = attackerMappings[currentAttacker]
    selectedOperator = currentAttacker
    Log()
end
 
function switchDefender()
    currentDefender = defenderMappings[currentDefender]
    selectedOperator = currentDefender
    Log()
end
 
function Log()
    if not IsKeyLockOn("scrolllock") then
        ClearLog()
        OutputLogMessage("Current mode: Attackers | Scroll lock is OFF\n\n")
OutputLogMessage("        (%s) | Ace            (%s) | Flores      (%s) | Jackal        (%s) | Sledge        \n        (%s) | Amaru          (%s) | Fuze        (%s) | Kali          (%s) | Thatcher      \n        (%s) | Ash            (%s) | Glaz        (%s) | Lion          (%s) | Thermite      \n        (%s) | Blackbeard     (%s) | Gridlock    (%s) | Maverick      (%s) | Twitch        \n        (%s) | Buck           (%s) | Grim        (%s) | Nokk          (%s) | Ying          \n        (%s) | Capitao        (%s) | Hibana      (%s) | Nomad         (%s) | Zero          \n        (%s) | Dokkaebi       (%s) | Iana        (%s) | Osa           (%s) | Zofia         \n        (%s) | Finka          (%s) | IQ          (%s) | Sens          \n        (%s) | Deimos         (%s) | Ram         (%s) | Brava         \n\n", 
            selectedOperator == "Ace", selectedOperator == "Flores", selectedOperator == "Jackal", selectedOperator == "Sledge", 
            selectedOperator == "Amaru", selectedOperator == "Fuze", selectedOperator == "Kali", selectedOperator == "Thatcher", 
            selectedOperator == "Ash", selectedOperator == "Glaz", selectedOperator == "Lion", selectedOperator == "Thermite", 
            selectedOperator == "Blackbeard", selectedOperator == "Gridlock", selectedOperator == "Maverick", selectedOperator == "Twitch", 
            selectedOperator == "Buck", selectedOperator == "Grim", selectedOperator == "Nokk", selectedOperator == "Ying", 
            selectedOperator == "Capitao", selectedOperator == "Hibana", selectedOperator == "Nomad", selectedOperator == "Zero", 
            selectedOperator == "Dokkaebi", selectedOperator == "Iana", selectedOperator == "Osa", selectedOperator == "Zofia",
            selectedOperator == "Finka", selectedOperator == "IQ", selectedOperator == "Sens", 
            selectedOperator == "Deimos", selectedOperator == "Ram", selectedOperator == "Brava")
 
 
    else
        ClearLog()
        OutputLogMessage("Current mode: Defenders | Scroll lock is ON\n\n")
        OutputLogMessage("Selected Operator:   %s\n\n", selectedOperator)
OutputLogMessage("        (%s) | Alibi        (%s) | Echo          (%s) | Maestro       (%s) | Smoke         (%s) | Tubarao\n        (%s) | Aruni        (%s) | Ela           (%s) | Melusi        (%s) | Tachanka      (%s) | Fenrir\n        (%s) | Azami        (%s) | Frost         (%s) | Mira          (%s) | Thorn         (%s) | Solis\n        (%s) | Bandit       (%s) | Goyo          (%s) | Mozzie        (%s) | Thunderbird   \n        (%s) | Castle       (%s) | Jager         (%s) | Mute          (%s) | Valkyrie      \n        (%s) | Caveira      (%s) | Kaid          (%s) | Oryx          (%s) | Vigil         \n        (%s) | Clash        (%s) | Kapkan        (%s) | Pulse         (%s) | Wamai         \n        (%s) | Doc          (%s) | Lesion        (%s) | Rook          (%s) | Warden        \n\n", -- Add other operators here
            selectedOperator == "Alibi", selectedOperator == "Echo", selectedOperator == "Maestro", selectedOperator == "Smoke", selectedOperator == "Tubarao",
            selectedOperator == "Aruni", selectedOperator == "Ela", selectedOperator == "Melusi", selectedOperator == "Tachanka", selectedOperator == "Fenrir",
            selectedOperator == "Azami", selectedOperator == "Frost", selectedOperator == "Mira", selectedOperator == "Thorn", selectedOperator == "Solis",
            selectedOperator == "Bandit", selectedOperator == "Goyo", selectedOperator == "Mozzie", selectedOperator == "Thunderbird", 
            selectedOperator == "Castle", selectedOperator == "Jager", selectedOperator == "Mute", selectedOperator == "Valkyrie", 
            selectedOperator == "Caveira", selectedOperator == "Kaid", selectedOperator == "Oryx", selectedOperator == "Vigil", 
            selectedOperator == "Clash", selectedOperator == "Kapkan", selectedOperator == "Pulse", selectedOperator == "Wamai",
            selectedOperator == "Doc", selectedOperator == "Lesion", selectedOperator == "Rook", selectedOperator == "Warden")
 
 
    end
end
 
function OnEvent(event, arg)
    EnablePrimaryMouseButtonEvents(true)
    if event == "MOUSE_BUTTON_PRESSED" then
        if arg == 3 and IsModifierPressed("lctrl") then
            if not IsKeyLockOn("scrolllock") then
                switchAttacker()
            else
                switchDefender()
            end
        elseif arg == 1 and IsMouseButtonPressed(3) and not IsKeyLockOn("capslock") then
            local operatorStats = operatorStats[selectedOperator]
            if operatorStats then
                for i = 1, #operatorStats, 3 do
                    local c_t = GetRunningTime()
                    local h_r = operatorStats[i]
                    local v_r = operatorStats[i+1]
                    local r_d = operatorStats[i+2]
                    repeat
                        local d_t = GetRunningTime() - c_t
                        MoveMouseRelative(h_r, v_r)
                        Sleep(10)
                    until d_t >= r_d or not IsMouseButtonPressed(1) or not IsMouseButtonPressed(3)
                    if not IsMouseButtonPressed(1) or not IsMouseButtonPressed(3) then break end
                end
            end
        end
    end
end
