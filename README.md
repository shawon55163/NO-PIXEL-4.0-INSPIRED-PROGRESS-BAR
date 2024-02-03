# This is QBCore Progressbar edited by me and inspired by NoPixel 4.0.

# Ox_lib integration tutorial DOWN HERE
### To integrate QBCore progressbar into ox_lib

- First go into ox_lib/resource/interface/client/progress.lua
- Then find **function lib.progressBar(data)**
- And then replace that whole function code with mine here

```
function lib.progressBar(data)
    while progress ~= nil do Wait(0) end

    if not interruptProgress(data) then
            playerState.invBusy = true
            exports['progressbar']:Progress({
            name = "random_task",
            duration = data.duration,
            label = data.label,
            useWhileDead = false,
            canCancel = true,
            controlDisables = {
                disableMovement = false,
                disableCarMovement = false,
                disableMouse = false,
                disableCombat = false,
            },
         }, function(cancelled)
            if not cancelled then
                -- finished
                --SendNUIMessage({
                    --action = 'progress',
                    --data = {
                        --label = data.label,
                        --duration = data.duration
                        --duration = -100
                    --}
                --})
                progress = nil
                playerState.invBusy = false
            else
                -- cancelled
                print("omg")
                -- Reset progress whether it's finished or cancelled
                progress = false
                Citizen.Wait(1000)
                progress = nil
                playerState.invBusy = false
            end
         end)

        return startProgress(data)
    end
end
```

# Progressbar

Dependency for creating progressbars in QB-Core.

# Usage

## QB-Core Functions

### Client

- QBCore.Functions.Progressbar(**name**: string, **label**: string, **duration**: number, **useWhileDead**: boolean, **canCancel**: boolean, **disableControls**: table, **animation**: table, **prop**: table, **propTwo**: table, **onFinish**: function, **onCancel**: function)
  > Create a new progressbar from the built in qb-core functions.<br>
  > **Example:**
  > ```lua
  >QBCore.Functions.Progressbar("random_task", "Doing something", 5000, false, true, {
  >    disableMovement = false,
  >    disableCarMovement = false,
  >    disableMouse = false,
  >    disableCombat = true,
  >}, {
  >    animDict = "mp_suicide",
  >    anim = "pill",
  >    flags = 49,
  >}, {}, {}, function()
  >    -- Done
  >end, function()
  >    -- Cancel
  >end)
  > ```

## Exports

### Client

- Progress(**data**: string, **handler**: function)
  > Creates a new progress bar directly from the export, always use the built in qb-core function if possible.<br>
  > **Example:**
  > ```lua
  >exports['progressbar']:Progress({
  >    name = "random_task",
  >    duration = 5000,
  >    label = "Doing something",
  >    useWhileDead = false,
  >    canCancel = true,
  >    controlDisables = {
  >        disableMovement = false,
  >        disableCarMovement = false,
  >        disableMouse = false,
  >        disableCombat = true,
  >    },
  >    animation = {
  >        animDict = "mp_suicide",
  >        anim = "pill",
  >        flags = 49,
  >    },
  >    prop = {},
  >    propTwo = {}
  >}, function(cancelled)
  >    if not cancelled then
  >        -- finished
  >    else
  >        -- cancelled
  >    end
  >end)
  > ```
  > **Props Example:**
  > ```lua
  >exports['progressbar']:Progress({
  >    name = "random_task",
  >    duration = 5000,
  >    label = "Doing something",
  >    useWhileDead = false,
  >    canCancel = true,
  >    controlDisables = {
  >        disableMovement = false,
  >        disableCarMovement = false,
  >        disableMouse = false,
  >        disableCombat = true,
  >    },
  >    animation = {
  >        animDict = "missheistdockssetup1clipboard@base",
  >        anim = "pill",
  >        flags = 49,
  >    },
  >    prop = {
  >      model = 'prop_notepad_01',
  >      bone = 18905,
  >      coords = vec3(0.1, 0.02, 0.05),
  >      rotation = vec3(10.0, 0.0, 0.0),
  >    },
  >    propTwo = {
  >      model = 'prop_pencil_01',
  >      bone = 58866,
  >      coords = vec3(0.11, -0.02, 0.001),
  >      rotation = vec3(-120.0, 0.0, 0.0),
  >    }
  >}, function(cancelled)
  >    if not cancelled then
  >        -- finished
  >    else
  >        -- cancelled
  >    end
  >end)
  > ```

  - isDoingSomething()
    > Returns a boolean (true/false) depending on if a progressbar is present.<br>
    > **Example:**
    > ```lua
    > local busy = exports["progressbar"]:isDoingSomething()
    > ```

