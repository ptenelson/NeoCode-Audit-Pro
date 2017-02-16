# NeoCode-Audit-Pro 
- it's so good you can take a 'nap' -

FileMaker Pro NOW has a native, plug-in free way to audit changes to your database.  This is a Technology Preview of NeoCode-Audit-Pro tool. It logs inserts, updates, and deletes to a separate FileMaker file without use of plug-ins. It is developed and tested on FileMaker Pro 15-- older versions are not supported.

FILES
* Task_Audit.fmp12 - Sample FileMaker Starter Solution that has had NeoCode-Audit-Pro integrated.
* NAP_Service.fmp12 - The Audit engine that pulls the logged event data out of Task_Audit.fmp12 and stores the events.

## FEATURES & LIMITATIONS
What the files will and will not do...

### FEATURE HIGHLIGHTS
* Logs 'Audit Events' when users Insert, Update and Delete
* Audit Events are logged to memory; NAP_Service then intermittently commits events to disk.
* Supports cascading delete & related record creation.
* Supports record revert.
* Task_Audit.fmp12 events are logged in memory. 
* NAP_Service.fmp12 manages garbage collection while committing events to disk.
* Uses ExecuteSQL() to dynamically capture database schema; changes to database design are captured.
* Minimal integration steps (to be documented).

### LIMITATIONS
Known bugs, incomplete and unsupported features.  This is essentially our to-do list. Not in any particular order.

* No implementation documentation.
* Does not support shared deployment (single user).
* Does not support FileMaker Go, FileMaker WebDirect (untested).
* Does not support server side data events (audits user events only).
* No error handling.
* Does not support multiple windows (untested).
* Does not log insert/update events triggered by importing data.
* Does not log container data-- yet.
* Does not support repeating fields.
* Does not support commas in data.
* Does not log table name.
* No implementation of rollback. (Does not support audit recovery)



## EXAMPLE
Excerpt of the custom function that logs audit events into global variables.
```
Let ([
	
    ...
	//####	JSON 	####//
	~json_sql_data  =_Aud_Get_AuditEvent (~QFN ; ~TblNum )

	];
	
	Case ( 
	//##### NO DATA  #####//
	IsEmpty ( ~json_sql_data ) ;  
	""  ; 

	//##### DELETE TYPE  #####//
	~Action  = "DELETE"  ; 
	_Aud_Set_AuditEvent ( ~json_sql_data ; ~Action ) ; 


	//##### INSERT  #####//
	~Action = "INSERT" ; 
	_Aud_Set_AuditEvent ( ~json_sql_data ; ~Action ) ; 
	

	//##### UPDATE  #####// 
	~Action = "UPDATE"  ; 
	_Aud_Set_AuditEvent ( ~json_sql_data ; ~Action ) ; 
	
	//##### DEFAULT  #####//

	"" 
	)
) //END Let
```

## INSTALLATION
### Requirements
    FileMaker Pro 15
    Tasks_Audit.fmp12 (this would be replaced by your file)
    NAP_Service.fmp12 (this is where your audit events are stored)
   
### Credentials
Opens automatically with the following credentials-- record changes CANNOT be logged using Admin (Full Access) account.

#### Audit Enabled (must be enabled to log events)
* user: "NAPUser"
* pass: ""

#### Full Access  (does not log)
* user: "Admin"
* pass: ""


## TESTS
1. Open Tasks_Audit.fmp12
1. Insert|Update|Delete records
1. From file menu, select "Window > Show Windows > NeoCode Audit Pro - Service"
1. Click the button "Show Audit Log"
1. Review that your record changes have been logged.

 
## USAGE

## REFERENCES
Andrew Duncan (DataBuzz) http://www.databuzz.com.au/using-executesql-to-query-the-virtual-schemasystem-tables/

## Contributors
* Paul Nelson
* Joshua Paul

## LICENSE
MIT
