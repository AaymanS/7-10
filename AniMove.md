-----------------------------------------------------------------------------------------
--
-- main.lua
--
-- Created by: Aayman Shameem
-- Created on: May 8, 2018
-- 
-- This code animates and makes the character move properly
-----------------------------------------------------------------------------------------

display.setStatusBar(display.HiddenStatusBar)
 
centerX = display.contentWidth * .5
centerY = display.contentHeight * .5

local sheetOptionsIdle =
{
    width = 232,
    height = 439,
    numFrames = 10
}
local sheetIdleNinja = graphics.newImageSheet( "./assets/spritesheets/ninjaBoyIdle.png", sheetOptionsIdle )

local sheetOptionsWalk =
{
    width = 363,
    height = 458,
    numFrames = 10
}
local sheetWalkingNinja = graphics.newImageSheet( "./assets/spritesheets/ninjaBoyJumpRun.png", sheetOptionsWalk )


local dPad = display.newImage( "./assets/sprites/d-pad.png" )
dPad.x = 150
dPad.y = display.contentHeight - 200
dPad.alpha = 0.50
dPad.id = "d-pad"


local rightArrow = display.newImage( "./assets/sprites/rightArrow.png" )
rightArrow.x = 260
rightArrow.y = display.contentHeight - 200
rightArrow.id = "right arrow"

local jumpButton = display.newImage( "./assets/sprites/jumpButton.png" )
jumpButton.x = display.contentWidth - 80
jumpButton.y = display.contentHeight - 80
jumpButton.id = "jump button"
jumpButton.alpha = 0.5



-- sequences table
local sequence_data = {
    -- consecutive frames sequence
    {
        name = "idle",
        start = 1,
        count = 10,
        time = 800,
        loopCount = 0,
        sheet = sheetIdleNinja
    },
    {
        name = "walk",
        start = 1,
        count = 10,
        time = 1000,
        loopCount = 1,
        sheet = sheetWalkingNinja
    }
}

local ninja = display.newSprite( sheetIdleNinja, sequence_data )
ninja.x = centerX - 650
ninja.y = centerY

ninja:play()

function rightArrow:touch( event )
    if ( event.phase == "ended" ) then
        -- move the character up
        transition.moveBy( ninja, { 
            x = 277, 
            y = 0, 
            time = 1000 
            } )
        ninja:setSequence( "walk" )
        ninja:play()
    end

    return true
end

-- reset to idle 
local function resetToIdle (event)
    if event.phase == "ended" then
        ninja:setSequence("idle")
        ninja:play()
    end
end

----------------------------------------------------------------------------------------------------------------

local sheetOptionsIdle2 =
{
    width = 567,
    height = 556,
    numFrames = 10
}
local sheetIdleRobot = graphics.newImageSheet( "./assets/spritesheets/robotIdle.png", sheetOptionsIdle2 )

local sheetOptionsWalk2 =
{
    width = 567,
    height = 556,
    numFrames = 8
}
local sheetJumpRobot = graphics.newImageSheet( "./assets/spritesheets/robotJump.png", sheetOptionsWalk2 )


-- sequences table
local sequence_data2 = {
    -- consecutive frames sequence
    {
        name = "idle",
        start = 1,
        count = 10,
        time = 800,
        loopCount = 0,
        sheet = sheetIdleRobot
    },
    {
        name = "jump",
        start = 1,
        count = 10,
        time = 1000,
        loopCount = 1,
        sheet = sheetJumpRobot
    }
    
}

local robot = display.newSprite( sheetIdleRobot, sequence_data2 )
robot.x = centerX + 650
robot.y = centerY

robot:play()

function jumpButton:touch( event )
    if ( event.phase == "ended" ) then
        -- make the character jump
        transition.moveBy (robot, {
        x = 0,
        y = -277,
        time = 1000 
        } )      

        robot:setSequence( "jump" )
        robot:play()
    
    end

    return true
end

-- reset to idle 
local function resetToIdle2 (event)
    if event.phase == "ended" then
        robot:setSequence("idle")
        robot:play()
    end
end


rightArrow:addEventListener( "touch", rightArrow )
ninja:addEventListener("sprite", resetToIdle)
robot:addEventListener("sprite", resetToIdle2)
jumpButton:addEventListener( "touch", jumpButton )
