# Crestron-Analytics
This module provides analytical analysis of events captured by a Crestron system to
examine the data and find outlying events that could be evidence of issues. This module
has specific uses for an automation system that is designed to assist with aging in place
where elderly people want to remain in their own home. However, it also has many uses
beyond that and can add significant value to an residential automation system as it adds
capabilities that have never been available before.
The Analytics v1 module is released as shareware. That is, it is free for Crestron
programmers use in their own automation systems. However, if you use this module in
customer systems where you, or the dealer/CSP you work for, will profit from it then I
ask for a single payment of $100.
What you get is
1. My thanks
2. Permission to use the modules on as many client projects as you want
3. A copy of the Simpl# source code for the Module
4. Good Karma
You can contact me at jay.m.basen@gmail.com to arrange for payment.
The module can collect and analyze four kinds of events
 Type 1 - Count trigger events during a specified period of time. For example, the
number of times a motion detector senses movement each hour during a day could
be collected. If there is a sudden increase, or decrease, in the number of triggers
during an hour, when compared to historical data that has been collected, the
module could trigger an alert to a care provider for an elderly person living in the
home that something could be wrong.
 Type 2 - Sample an analog value on a timed interval - For example, the system
could sample the inside temperature of a home every half hour. If the temperature
varies, when compared to the historical data that has been collected, the module
could trigger an alert to a care provider for an elderly person living in the home
that the person could be in danger
 Type 3 - Time length of an event vs. correlated data. - For example, the module
could track the run time of a furnace in the winter when compared to the
difference between the thermostat set point and the outside temperature. As the
gap between the set point and the outside temperature increases (either due to the
set point being raised or the outside temperature dropping in the winter) the time
it takes to heat the home will increase. The module will track the specific, unique
data for the home and trigger an event when the time to heat the home increases
beyond expected norms as this could be evidence of an impending furnace failure.
The event could be targeted to the homeowner, a care taker, or both. 
 Type 4 - Cumulative time of an activity during a set period of time. For example,
the module could track the amount of time, each day, that a person spends in bed
by connecting a pressure sensor in the bed to the automation processor / hub. If
the homeowner suddenly spends more time in bed than is expected based on the
data the module has collected it can trigger an alert to a care provider that there
may be an issue with the health of the homeowner.
These are just examples of how the module could be used to collect data and add value to
an automation system. The possibilities are endless.
Data from the module is saved in an XML data file so if the processor has to be rebooted
the data will be read by the module and analysis of new events can continue without
interruption. The file can be located in either NVRAM or on removable media (RM).
Multiple instances of this module can be inserted into a single SimplWindows program.
In this way a single program can monitor a wide variety of events, simultaneously
Parameters
The parameters on the module drive how the module will operate.
XML_Filename - name of xml file that will contain the collected data
XML_Path - Location of the XML data file, either "NVRAM" or "RM"
Analytics_Type - The type, as described above of data collection and analysis
that will be done by the module. Choices are: "Timed Trigger Count", "Timed
Analog Sample", "Value vs. Correlated Data", and "Cumulative Time of
Activity". This choice will drive which set of inputs should be used during data
collection.
Logger_Level - level of logger logging that will be done by the Simpl# code.
Options are: "None", "Errors", and "Errors + Status". Unless you are specifically
debugging issues with the module this should be set to "None".
Data_Count - The number of events that the system will capture and maintain for
analysis. For example, the Analytics_Type is set to "Cumulative Time of
Activity" (Type 4), the Data_Count is set to thirty, and the Time_Between_Calcs
parameter is set to "1 Day", then the system will collect thirty days of data. After
thirty days the system will start to throw away the oldest data as it collects new
data.
Time_Between_Calcs - Specifies the time period of data collection as described
in the above description of the Data_Count parameter. Choices are: "1 Minute",
"5 Minutes", "15 Minutes", "30 Minutes", "1 Hour", "4 Hours", "8 Hours", and "1
Day".
Use_Occupancy - This parameter will tell the module to user, or ignore, the
Number_Of_Occupants analog input signal. If Use_Occupancy is set to "Use
Occupancy in Analytics" then the analog value of the Number_Of_Occupants
analog input is assumed to be the number of people in the home and will be used
to normalize the data collected; if appropriate based on the Analytics_Type. For 
example, if the Analytics_Type is set to "Timed Trigger Count" and the
Number_Of_Occupants analog input is set to two, then all data collected will be
normalized to one occupant by dividing it by two.
Inputs
Init - This digital input must be pulsed at startup to initialize the module. The
module will, if available, read the xml data file and analyze the data so that data
collected from that point forward can be compared to the data collected and
outputs can be triggered if the data doesn't fall within the expected bounds.
Number_Of_Occupants - This analog input operates as described within the
description of the Use_Occupancy parameter. In addition, if the value is set to
zero the residence is assumed to be unoccupied. Any data collected while the
residence is unoccupied is discarded. For example, if Analytics_Type is set to
"Timed Analog Sample" and the Number_Of_Occupants is set to zero for part of
the time period specified by the Time_Between_Calcs parameter then all the data
collected during that time period is discarded as it would be impossible to
compare this data with data collected during other time periods without
fabricating data during the unoccupied period. This module will never fabricate
data.
Dump_Data_To_Logger - Pulsing this digital input will dump all the working
data to logger for analysis during debugging.
The remainder of the inputs to the module are for collection of data. There are
separate input signals for each Analytics_Type.
Type 1 - Timed Trigger Count
Type_1_Start_Collection - Pulsed to begin the timed collection of data
Type_1_Y_Increment - Pulsed every time there is an event that is being
monitored.
Type 2 - Timed Analog Sample
Type_2_Start_Collection - Pulsed to begin the timed collection of data
Type_2_Y_Value - Analog value being collected at the end of each time
interval specified by the Time_Between_Calcs parameter.
Type 3 - Value vs. Correlated Data
Type_3_Start_Collection - Pulsed to begin the timed collection of data
Type_3_X_Value, Type_3_X_Diff_1, Type_3_X_Diff_2 - These input will
be used to collect the data that the Y input is correlated against. The data can
either be a simple analog value that is entered on the Type_3_X_Value signal 
or can be entered using the calculated difference between Type_3_X_Diff_1
and Type_3_X_Diff_2 analog inputs.
Type_3_Start_Event - Pulsed to start the timed portion of the correlated data
Type_3_Stop_Event - Pulsed to stop the timed portion of the correlated data
Type 4 - Cumulative Time of Activity
Type_4_Start_Collection - Pulsed to begin the timed collection of data
Type_4_Start_Event - Pulsed to start each event captured during the time
specified in the Time_Between_Calcs parameter
Type_4_Stop_Event - Pulsed to stop each event captured during the time
specified in the Time_Between_Calcs parameter
Outputs
Out_By_1_Standard_Deviation - High if the newly collected data is more than one
standard deviation away from the expected value
Out_By_2_Standard_Deviation - High if the newly collected data is more than two
standard deviations away from the expected value
Out_By_3_Standard_Deviation - High if the newly collected data is more than three
standard deviations away from the expected value
Out_By_4_Standard_Deviation - High if the newly collected data is more than four
standard deviations away from the expected value
Status$ - Status information from the module related to the analytical calculations
performed. A single round of calculations will produce multiple string outputs so this
isn't appropriate for display on a touch panel. However, it is useful for debugging when
displayed in the toolbox debugger. The strings are much briefer than those that will be
displayed by the logger when the Logger_Level parameter is set to "Errors + Status" 
