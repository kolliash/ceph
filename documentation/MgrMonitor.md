MgrMonitor.cc Module Documentation

Overview:
The MgrMonitor class is an importan part of ceph, it handles the monitoring and management of manager daemons within the cluster. The class is responsible for maintainging information about active and sstandby mgr daemons, processing commands, and interacting with other parts of the cluster. The overall purpose is that the configuration updates accurately and sucessfully. 

Main Purpose: 
- Managing active and standby mgr daemons.
- Handling mgr command processing and execution.
- Managing the lifecycle of mgr modules.
- enabling/disabling mgr modules.

Class and Function Descriptions
_________________________________
Data Members:
There are multiple data members, each serving different aspects of managing the state and health of mgr daemons in Ceph. Below are a few important ones that are used frequently and store important information. 

    1. MgrMap map: Current state of mgr daemons, including active and standby managers.
    2. MgrMap pending_map: Temporary copy of map used for staging updates.
    3. bool ever_had_active_mgr: Tracks if an active mgr has ever been present.
    4. std::map<std::string, ceph::buffer::list> pending_metadata: Metadata waiting to be added or updated.
    5. std::set<std::string> pending_metadata_rm: Metadata entries pending removal.
    6. std::map<std::string, Option> mgr_module_options: Configurations for mgr modules.
    7. std::list<std::string> misc_option_strings: Extra strings related to mgr options.
    8. utime_t first_seen_inactive: Time when mgr was first seen as inactive.
    9. std::map<uint64_t, ceph::coarse_mono_clock::time_point> last_beacon: Last heartbeat time from each mgr daemon.

Core Functions:
There are a lot of functions in the code but the ones listed below are important in extending the output of ceph mgr module ls -f json-pretty, which is the part of the code we wanted to fix. The relevent functions to understand are listed below.

    1. preprocess_command: This function handles incoming commands and checks if they match the "mgr module ls" prefix. Here, you determine what kind of data should be formatted and returned, including the details of enabled and always-on modules.
    2. prepare_command: This function processes commands like "mgr module ls" and formats the response using a Formatter object. For your task, you would extend this part to ensure the output for enabled and always-on modules is detailed, similar to the output for disabled modules.
    3. MgrMonitor::get_map and MgrMonitor::map: This structure holds the state of various mgr modules, including the information about enabled and always-on modules. You would extract and format this data in the relevant command handling sections.
    4. MgrMap::get_always_on_modules and MgrMap::available_modules: These provide access to the lists of always-on and available modules. They will help differentiate the modules that need to be included in the detailed output.
