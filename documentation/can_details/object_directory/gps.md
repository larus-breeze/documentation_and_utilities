GPS
===

    Name                                         gps                                
    Object Id                                    3                                  
    Preffered IDs for Specific Datapoints        0x140 - 0x14f                      
    Preffered IDs for Generic Datapoints         0x540 - 0x54f                      
    Comment                                      Definition of CAN bus datagrams of a GPS Receiver

ID 0x140 Date and Time
---
Name: date_time  
Object-ID Version: 0  
Type: Data Object  
Interval: 100 ms  
Length: 7 Bytes  
Dynamic Id: Id(Heartbeat) - 0x400 + 0x00

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   year                     u16       year                                         
    2   month                    u8        month                                        
    3   day                      u8        day                                          
    4   hour                     u8        h                                            
    5   minute                   u8        min                                          
    6   second                   u8        s                                            

ID 0x141 Latitude
---
Name: latitude  
Object-ID Version: 0  
Type: Data Object  
Interval: 100 ms  
Length: 4 Bytes  
Dynamic Id: Id(Heartbeat) - 0x400 + 0x01

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   latitude                 f64       rad                                          

ID 0x142 Longitude
---
Name: longitude  
Object-ID Version: 0  
Type: Data Object  
Interval: 100 ms  
Length: 4 Bytes  
Dynamic Id: Id(Heartbeat) - 0x400 + 0x02

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   longitude                f64       rad                                          

ID 0x143 MSL Altitude and Geo Separation
---
Name: altitude  
Object-ID Version: 0  
Type: Data Object  
Interval: 100 ms  
Length: 8 Bytes  
Dynamic Id: Id(Heartbeat) - 0x400 + 0x03

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   altitude                 f32       m                                            
    4   geo_separation           f32       m                                            

ID 0x144 Ground Track and Ground Speed
---
Name: track_speed  
Object-ID Version: 0  
Type: Data Object  
Interval: 100 ms  
Length: 8 Bytes  
Dynamic Id: Id(Heartbeat) - 0x400 + 0x04

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   gnd_track                f32       rad                                          
    4   gnd_speed                f32       m/s                                          

ID 0x145 Number of Sattelites and Sat Fix Type
---
Name: satellites  
Object-ID Version: 0  
Type: Data Object  
Interval: 100 ms  
Length: 2 Bytes  
Dynamic Id: Id(Heartbeat) - 0x400 + 0x05

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   number_of_sats           u8        .                                            
    1   fix_type                 u8        0 no_gps                                     
                                           1 2d_fix                                     
                                           2 3d_fix                                     
                                           3 rtk                                        

ID 0x540 Heartbeat
---
Name: heartbeat  
Object-ID Version: 0  
Type: Data Object  
Interval: 1000 ms  
Length: 8 Bytes  
Dynamic Id: Id(Heartbeat) + 0x00

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   object_id                u16       Enumeration, see CAN Specificatiion          
    2   object_id_generation     u16       Generation of Object Id                      
    4   dev_uid                  u8[4]     DevUid                                       

ID 0x541 Hardware and Firmware Version
---
Name: version  
Object-ID Version: 0  
Type: Data Object  
Interval: if required by remote frame request, check exact id  
Length: 8 Bytes  
Dynamic Id: Id(Heartbeat) + 0x01

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   hw_version               HwVersion .                                            
    4   fw_version               FwVersion .                                            

ID 0x542 Set System Wide Config Item
---
Name: set_config  
Object-ID Version: 0  
Type: Service  
Interval: if required  
Length: 8 Bytes  
Dynamic Id: Id(Heartbeat) + 0x02

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   config_id                u16       Enumeration                                  
                                           0: volume_vario                              
                                           1: mac_cready                                
                                           2: water_ballast                             
                                           3: bugs                                      
                                           4: qnh                                       
                                           5: pilot_weight                              
                                           6: vario_mode_control                        
                                           7: tc_climb_rate                             
                                           8: tc_speed_to_fly                           
    2   config_data              u8[6]     Specific to the item                         
                                           0: volume_vario:       u8 unit db, u8[5] reserved
                                           1: mac_cready:         u8[2] reserved, f32 unit m/s
                                           2: water_ballast:      u8[2] reserved, f32 unit fraction
                                           3: bugs:               u8[2] reserved, f32 unit factor
                                           4: qnh:                u8[2] reserved, f32 unit Pa
                                           5: pilot_weight:       u8[2] reserved, f32 unit kg
                                           6: vario_mode_control  u8: 0 Vario, 1 SpeedToFly, 2 Auto
                                           7: tc_climb_rate:      u8[2] reserved, f32 unit s
                                           8: tc_speed_to_fly:    u8[2] reserved, f32 unit s

ID 0x543 Transfer of Binary Data Blocks
---
Name: blob_upload  
Object-ID Version: 0  
Type: Service  
Interval: if required  
Length: 8 Bytes  
Dynamic Id: Id(Heartbeat) + 0x03

    No  Datapoint                Type      Unit / Comment                               
    --------------------------------------------------------------------------------------------
    0   upload_state             u3        0 type_of_transmission                       
                                           1 length                                     
                                           2 crc                                        
                                           3 data                                       
                                           4 last data                                  
                                           5 ack_transmission                           
    0.3 block_count              u5        block counter                                
    1   content                  u8[7]     upload_state 0: u8[3] reserved (u8=0), u32 type_of_transmission
                                           upload_state 1: u8[3] reserved (u8=0), u32 blob_length
                                           upload_state 2: u8[3] reserved (u8=0), u32 crc
                                           upload_state 3: u8[7] blob data              
                                           upload_state 4: u8[7] blob data              
                                           upload_state 5: u8[3] reserved (=0), u32 requested_block
                                           _                                            
                                           Type of transmission:                        
                                           0 NMEA GPS data                              
                                           1 NMEA Flarm data                            
                                           2 Larus configuration data                   
                                           3 Larus firmware image                       

