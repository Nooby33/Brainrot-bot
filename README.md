# Brainrot-bot
A chatbot with memes for you friend's to enjoy and laugh 

local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Create Screen GUI
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.ResetOnSpawn = false

-- Create Main Frame
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 550, 0, 550)
MainFrame.Position = UDim2.new(0.5, -275, 0.5, -275)
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
MainFrame.Active = true
MainFrame.Draggable = true

-- Execute Button
local ExecuteButton = Instance.new("TextButton", MainFrame)
ExecuteButton.Size = UDim2.new(0, 80, 0, 30)
ExecuteButton.Position = UDim2.new(0.05, 0, 0.1, 0)
ExecuteButton.Text = "Execute"
ExecuteButton.BackgroundColor3 = Color3.fromRGB(60, 180, 75)

-- Close Button
local CloseButton = Instance.new("TextButton", MainFrame)
CloseButton.Size = UDim2.new(0, 80, 0, 30)
CloseButton.Position = UDim2.new(0.85, -10, 0.1, 0)
CloseButton.Text = "Close"
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)

-- Save Button
local SaveButton = Instance.new("TextButton", MainFrame)
SaveButton.Size = UDim2.new(0, 100, 0, 30)
SaveButton.Position = UDim2.new(0.7, 0, 0.1, 0)
SaveButton.Text = "Save Script"
SaveButton.BackgroundColor3 = Color3.fromRGB(59, 209, 121)

-- Clear Code Button
local ClearButton = Instance.new("TextButton", MainFrame)
ClearButton.Size = UDim2.new(0, 100, 0, 30)
ClearButton.Position = UDim2.new(0.35, 0, 0.1, 0)
ClearButton.Text = "Clear Code"
ClearButton.BackgroundColor3 = Color3.fromRGB(200, 150, 0)

-- ScriptBox
local ScriptBox = Instance.new("TextBox", MainFrame)
ScriptBox.Size = UDim2.new(0, 480, 0, 100)
ScriptBox.Position = UDim2.new(0.05, 0, 0.25, 0)
ScriptBox.PlaceholderText = "Enter Lua code here..."
ScriptBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ScriptBox.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Output Label
local OutputLabel = Instance.new("TextLabel", MainFrame)
OutputLabel.Size = UDim2.new(0, 480, 0, 30)
OutputLabel.Position = UDim2.new(0.05, 0, 0.55, 0)
OutputLabel.Text = "Output will appear here."
OutputLabel.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
OutputLabel.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Saved Scripts (1, 2)
local SavedScripts = {nil, nil}

-- Run Script Buttons
local function createSavedScriptButton(index)
    local Button = Instance.new("TextButton", MainFrame)
    Button.Size = UDim2.new(0, 120, 0, 30)
    Button.Position = UDim2.new(0.05 + (index - 1) * 0.45, 0, 0.65, 0)
    Button.Text = "Run Script " .. index
    Button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    Button.MouseButton1Click:Connect(function()
        if SavedScripts[index] then
            local code = SavedScripts[index]
            local success, result = pcall(function()
                return loadstring(code)()
            end)
            if success then
                OutputLabel.Text = "Executing Saved Script " .. index .. ": " .. tostring(result)
            else
                OutputLabel.Text = "Error in Script " .. index .. ": " .. tostring(result)
            end
        else
            OutputLabel.Text = "No script saved in Run Script " .. index
        end
    end)
    return Button
end

local RunScript1 = createSavedScriptButton(1)
local RunScript2 = createSavedScriptButton(2)

-- Execute Script Function
local function executeScript()
    local code = ScriptBox.Text
    local success, result = pcall(function()
        return loadstring(code)()
    end)
    if success then
        OutputLabel.Text = "Output: " .. tostring(result)
    else
        OutputLabel.Text = "Error: Incorrect script or not found."
    end
end

-- Save Script Function
local function saveScript()
    local code = ScriptBox.Text
    for i = 1, 2 do
        if not SavedScripts[i] then
            SavedScripts[i] = code
            OutputLabel.Text = "Script saved in Run Script " .. i
            return
        end
    end
    OutputLabel.Text = "Error: All slots are occupied."
end

-- Clear Code Function
local function clearCode()
    ScriptBox.Text = ""
    OutputLabel.Text = "Script cleared."
end

-- Connect Buttons
ExecuteButton.MouseButton1Click:Connect(executeScript)
SaveButton.MouseButton1Click:Connect(saveScript)
ClearButton.MouseButton1Click:Connect(clearCode)

-- Close Functionality
CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
end)

-- Chatbot Section
local ChatBox = Instance.new("TextBox", MainFrame)
ChatBox.Size = UDim2.new(0, 480, 0, 30)
ChatBox.Position = UDim2.new(0.05, 0, 0.75, 0)
ChatBox.PlaceholderText = "Ask the chatbot anything..."
ChatBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ChatBox.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Chatbot Logic
local function chatbotResponse(message)
    local response = "I'm here to help! Ask about scripts or commands."

    if message:find("hi") or message:find("hello") then
        response = "Hello! How can I assist?"
    elseif message:find("script") then
        response = "Ask me about saved scripts with 'run script 1' or 'run script 2'."
    elseif message == "1+1" then
        response = "The answer is 2."
    end

    OutputLabel.Text = response
end

ChatBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        chatbotResponse(ChatBox.Text:lower())
        ChatBox.Text = ""
    end
end)

-- Reset Chatbot Button
local ResetChatbotButton = Instance.new("TextButton", MainFrame)
ResetChatbotButton.Size = UDim2.new(0, 120, 0, 30)
ResetChatbotButton.Position = UDim2.new(0.05, 0, 0.85, 0)
ResetChatbotButton.Text = "Reset Chatbot"
ResetChatbotButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)

ResetChatbotButton.MouseButton1Click:Connect(function()
    OutputLabel.Text = "Chatbot reset. Ask a new question."
end)

-- Warning Label
local WarningLabel = Instance.new("TextLabel", MainFrame)
WarningLabel.Size = UDim2.new(0, 480, 0, 30)
WarningLabel.Position = UDim2.new(0.05, 0, 0.9, 0)
WarningLabel.Text = "⚠️ Only reset chatbot if having issues."
WarningLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
WarningLabel.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
