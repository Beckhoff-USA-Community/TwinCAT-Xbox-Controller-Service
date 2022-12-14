
# NC Helper Functions

This documentation is to serve as a reference for the NC Helper function blocks that are included with the XboxControllerUtilities PLC library. Please visit the Getting Started section for the installation of the PLC library.

## <span style="color:red">CAUTION!</span>
The blocks below use Move Velocity commands under the hood, <span style="color:red">use at your own risk</span>. Internally, they will copy the original Feedrate Override value of the Axis, and then supply a new one based on the Options Structure provided. Once the bEnable bit turns False, the original Feedrate Override value is copied back into the Axis and it is then halted. The feedrate copy feature is done to allow a smooth ramping based on the force applied to each joystick or trigger.

## Button Based Helpers

The button based helpers can work in two ways; a simple instant jog button, or a rampable jog button. When enabling the ramp feature inside the ST_Xbox_Button_NC_Options struct, the axis will ramp to a specified value based on how long the button is pressed.

### **FB_Xbox_Dual_Button_NC**

The dual button block uses two buttons to jog the axis backward and forward.

```javascript
VAR
    // FB_Xbox_Dual_Button_NC Instance
    fbXbox_Dual_Button_NC : FB_Xbox_Dual_Button_NC;

    // Xbox Controller Instance
    XboxController : FB_Xbox_Controller(NetID := '', iControllerNumber := 1);

    // Enable for the Jog command
    bEnable				  : BOOL;

    // Axis Reference to NC
	Axis_1				  : Axis_Ref;

    // Options for the Jog command
	stOptions			  : ST_Xbox_Button_NC_Options;
END_VAR
```
```javascript
// Update IO
XboxController.Cycle();

// Using FB_Xbox_Dual_Button_NC to control an NC Axis
fbXbox_Dual_Button_NC.Jog(bEnable          := bEnable AND XboxController.P_Status.bConnected,
                          bForward_Button  := XboxController.P_Buttons.X,
                          bBackward_Button := XboxController.P_Buttons.Y,
                          Axis		       := Axis_1,
                          stOptions        := stOptions);
```

| Parameter         | Type                      | Description                                                                                                                                                      |
| --------------    | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bEnable		    | BOOL                      | Enable the jog command. It will halt the axis on falling edge.                                                                                                   |
| bForward_Button   | BOOL                      | Command value to jog the axis forward                                                                                                                            |
| bBackward_Button	| BOOL                      | Command value to jog the axis backward                                                                                                                           |
| Axis				| REFERENCE TO Axis_Ref     | Axis_Ref for the NC Axis to Jog                                                                                                                                  |
| stOptions			| ST_Xbox_Button_NC_Options | Options structure for ramping features and inverting the direction of jogging                                                                                    |


### **FB_Xbox_Single_Button_NC**

The single button block uses only one buttons to jog the axis in a specific direction. The direction of jog is based on the stOptions struct settings and the invert direction bit.

```javascript
VAR
    // FB_Xbox_Single_Button_NC Instance
    fbXbox_Single_Button_NC : FB_Xbox_Single_Button_NC;

    // Xbox Controller Instance
    XboxController : FB_Xbox_Controller(NetID := '', iControllerNumber := 1);
    
    // Enable for the Jog command
    bEnable				  : BOOL;

    // Axis Reference to NC
	Axis_1				  : Axis_Ref;

    // Options for the Jog command
	stOptions			  : ST_Xbox_Button_NC_Options;
END_VAR
```
```javascript
// Update IO
XboxController.Cycle();

// Using FB_Xbox_Single_Button_NC to control an NC Axis
fbXbox_Single_Button_NC.Jog(bEnable   := bEnable AND XboxController.P_Status.bConnected,
                            bButton   := XboxController.P_Buttons.X,
                            Axis      := Axis_1,
                            stOptions := stOptions);
```

| Parameter         | Type                      | Description                                                                                                                                                      |
| --------------    | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bEnable		    | BOOL                      | Enable the jog command. It will halt the axis on falling edge.                                                                                                   |
| bButton       	| BOOL                      | Command value to jog the axis (direction is based on the stOptions)                                                                                              |
| Axis				| REFERENCE TO Axis_Ref     | Axis_Ref for the NC Axis to Jog                                                                                                                                  |
| stOptions			| ST_Xbox_Button_NC_Options | Options structure for ramping features and inverting the direction of jogging                                                                                    |

---

## Joystick Based Helpers

### **FB_Xbox_2D_Joystick_NC**

The 2D joystick block uses both the X and Y value of the controller to drive two different axis. This is extremely helpful when working with a cartesian robot using XY coordinates.

```javascript
VAR
    // FB_Xbox_2D_Joystick_NC Instance
    fbXbox_2D_Joystick_NC : FB_Xbox_2D_Joystick_NC;

    // Xbox Controller Instance
    XboxController : FB_Xbox_Controller(NetID := '', iControllerNumber := 1);

    // Enable for the Jog command
    bEnable				  : BOOL;

    // Axis Reference to NC
	X_Axis				  : Axis_Ref;
    Y_Axis				  : Axis_Ref;

    // Options for the Jog command
	stOptions			  : ST_Xbox_2D_Joystick_NC_Options;
END_VAR
```
```javascript
// Update IO
XboxController.Cycle();

// Using FB_Xbox_2D_Joystick_NC to control an NC Axis
fbXbox_2D_Joystick_NC.Jog(bEnable     := bEnable AND XboxController.P_Status.bConnected,
                                        stJoystick  := XboxController.P_Left_Joystick,
                                        X_Axis      := X_Axis,
                                        Y_Axis      := Y_Axis,
                                        stOptions   := stOptions);
```

| Parameter         | Type                                          | Description                                                                                                                                                      |
| --------------    | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bEnable		    | BOOL	                                        | Enable the jog command. It will halt the axis on falling edge.                                                                                                   |
| stJoystick	    | REFERENCE TO ST_Xbox_Controller_Joystick      | Joystick from the controller                                                                                                                                     |
| X_Axis		    | REFERENCE TO Axis_Ref                         | Axis to follow the X direction of the Joystick                                                                                                                   |
| Y_Axis		    | REFERENCE TO Axis_Ref                         | Axis to follow the Y direction of the Joystick                                                                                                                   |
| stOptions	        | ST_Xbox_2D_Joystick_NC_Options  | Options structure                                                                                                                                                |

### **FB_Xbox_1D_Joystick_NC**

The 1D joystick block uses only one joystick value of the controller to drive a single axis.

```javascript
VAR
    // FB_Xbox_1D_Joystick_NC Instance
    fbXbox_1D_Joystick_NC : FB_Xbox_1D_Joystick_NC;

    // Xbox Controller Instance
    XboxController : FB_Xbox_Controller(NetID := '', iControllerNumber := 1);

    // Enable for the Jog command
    bEnable				  : BOOL;

    // Axis Reference to NC
	Axis_1				  : Axis_Ref;

    // Options for the Jog command
	stOptions			  : ST_Xbox_1D_Joystick_NC_Options;
END_VAR
```
```javascript
// Update IO
XboxController.Cycle();

// Using FB_Xbox_1D_Joystick_NC to control an NC Axis
fbXbox_1D_Joystick_NC.Jog(bEnable   := bEnable AND XboxController.P_Status.bConnected,
                                          stJoystick  := XboxController.P_Left_Joystick.fX,
                                          Axis        := Axis_1,
                                          stOptions   := stOptions);
```

| Parameter         | Type                                          | Description                                                                                                                                                      |
| --------------    | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bEnable		    | BOOL	                                        | Enable the jog command. It will halt the axis on falling edge.                                                                                                   |
| fJoystick	        | LREAL                                         | Joystick value from the controller                                                                                                                               |
| Axis		        | REFERENCE TO Axis_Ref                         | Axis to follow the direction of the Joystick                                                                                                                     |
| stOptions	        | ST_Xbox_1D_Joystick_NC_Options| Options structure                                                                                                                                                |

---

## Trigger Based Helpers

### **FB_Xbox_Dual_Trigger_NC**

The dual trigger joystick block uses both left and right triggers of the controller to drive a single axis.

```javascript
VAR
    // FB_Xbox_Dual_Trigger_NC Instance
    fbXbox_Dual_Trigger_NC : FB_Xbox_Dual_Trigger_NC;

    // Xbox Controller Instance
    XboxController : FB_Xbox_Controller(NetID := '', iControllerNumber := 1);

    // Enable for the Jog command
    bEnable				  : BOOL;

    // Axis Reference to NC
	Axis_1				  : Axis_Ref;

    // Options for the Jog command
	stOptions			  : ST_Xbox_Trigger_NC_Options;
END_VAR
```
```javascript
// Update IO
XboxController.Cycle();

// Using FB_Xbox_Dual_Trigger_NC to control an NC Axis
fbXbox_Dual_Trigger_NC.Jog(bEnable        := bEnable AND XboxController.P_Status.bConnected,
                           fLeft_Trigger  := XboxController.P_Left_Trigger,
                           fRight_Trigger := XboxController.P_Right_Trigger,
                           Axis           := Axis_1,
                           stOptions      := stOptions);
```

| Parameter         | Type                                          | Description                                                                                                                                                      |
| --------------    | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bEnable       	| BOOL	                                        | Enable the jog command. It will halt the axis on falling edge.                                                                                                   |
| fLeft_Trigger 	| LREAL                                         | Left trigger value from the controller                                                                                                                           |
| fRight_Trigger	| LREAL                                         | Right trigger value from the controller                                                                                                                          |
| Axis          	| REFERENCE TO Axis_Ref                         | Axis to follow the trigger commands                                                                                                                              |
| stOptions         | ST_Xbox_Trigger_NC_Options                    | Options structure                                                                                                                                                |


### **FB_Xbox_Single_Trigger_NC**

The single trigger joystick block uses only one of the controller triggers to drive a single axis. The direction of jog is based on the stOptions struct settings and the invert direction bit.

```javascript
VAR
    // FB_Xbox_Single_Trigger_NC Instance
    fbXbox_Single_Trigger_NC : FB_Xbox_Single_Trigger_NC;

    // Xbox Controller Instance
    XboxController : FB_Xbox_Controller(NetID := '', iControllerNumber := 1);

    // Enable for the Jog command
    bEnable				  : BOOL;

    // Axis Reference to NC
	Axis_1				  : Axis_Ref;

    // Options for the Jog command
	stOptions			  : ST_Xbox_Trigger_NC_Options;
END_VAR
```
```javascript
// Update IO
XboxController.Cycle();

// Using FB_Xbox_Single_Trigger_NC to control an NC Axis
fbXbox_Single_Trigger_NC.Jog(bEnable   := bEnable AND XboxController.P_Status.bConnected,
                             fTrigger  := XboxController.P_Left_Trigger,
                             Axis      := Axis_1,
                             stOptions := stOptions);
```

| Parameter         | Type                                          | Description                                                                                                                                                      |
| --------------    | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bEnable       	| BOOL	                                        | Enable the jog command. It will halt the axis on falling edge.                                                                                                   |
| fTrigger 	        | LREAL                                         | Trigger value from the controller                                                                                                                           |
| Axis          	| REFERENCE TO Axis_Ref                         | Axis to follow the trigger commands                                                                                                                              |
| stOptions         | ST_Xbox_Trigger_NC_Options                    | Options structure                                                                                                                                                |

---


## DUTs

### **ST_Xbox_Button_NC_Options**

| Name                    | Type                      | Default Value | Description                                                           |
| ----------------------- | ------------------------- | ------------- | --------------------------------------------------------------------- |
| bInverted               | BOOL                      | FALSE         | Inverts the direction of the axis                                     |
| fMaxOverride            | LREAL                     | 100.0         | Maximum Feedrate                                                      |
| bRampedOverride         | BOOL                      | TRUE          | Ramped Feedrate - Increase the feedrate over time of button pressed   |
| tRampStepTime           | TIME                      | T#100MS       | Ramp Step - How often to increase override as button is pressed       |
| iRampStepFactor         | INT                       | 5             | Ramp Step Factor - The % override to step at each time period         |
| stXbox_NC_Halt_Dynamics | ST_Xbox_NC_Halt_Dynamics  |               | Struct of parameters for MC_Halt command                              |


### **ST_Xbox_2D_Joystick_NC_Options**

| Name                      | Type                      | Default Value | Description                                                           |
| ------------------------- | ------------------------- | ------------- | --------------------------------------------------------------------- |
| bInverted_X               | BOOL                      | FALSE         | Inverts the direction of the X axis                                   |
| bInverted_Y               | BOOL                      | FALSE         | Inverts the direction of the Y axis                                   |
| fMaxOverride_X            | LREAL                     | 100.0         | Maximum Feedrate X Axis                                               |
| fMaxOverride_Y            | LREAL                     | 100.0         | Maximum Feedrate Y Axis                                               |
| stXbox_NC_Halt_Dynamics   | ST_Xbox_NC_Halt_Dynamics  |               | Struct of parameters for MC_Halt command                              |


### **ST_Xbox_1D_Joystick_NC_Options**

| Name                    | Type                      | Default Value | Description                              |
| ----------------------- | ------------------------- | ------------- | ---------------------------------------- |
| bInverted               | BOOL                      | FALSE         | Inverts the direction of the axis        |
| fMaxOverride            | LREAL                     | 100.0         | Maximum Feedrate                         |
| stXbox_NC_Halt_Dynamics | ST_Xbox_NC_Halt_Dynamics  |               | Struct of parameters for MC_Halt command |


### **ST_Xbox_Trigger_NC_Options**


| Name                    | Type                      | Default Value | Description                              |
| ----------------------- | ------------------------- | ------------- | ---------------------------------------- |
| bInverted               | BOOL                      | FALSE         | Inverts the direction of the axis        |
| fMaxOverride            | LREAL                     | 100.0         | Maximum Feedrate                         |
| stXbox_NC_Halt_Dynamics | ST_Xbox_NC_Halt_Dynamics  |               | Struct of parameters for MC_Halt command |


### **ST_Xbox_NC_Halt_Dynamics**

| Name              | Type                        | Default Value | Description                        |
| ----------------- | --------------------------- | ------------- | ---------------------------------- |
| fDeceleration      | LREAL                      | 5000          | Deceleration of the MC_Halt command|
| fJerk              | LREAL                      | 10000         | Jerk of the MC_Halt command        |
| mcBufferMode       | MC_BufferMode              |               | BufferMode of the MC_Halt command  |
| stOptions          | ST_MoveOptions             |               | Options oif the MC_Halt command    |