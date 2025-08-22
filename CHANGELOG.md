# HAMMER Changelog

## v2025.8.4 - Entity Auto-Discovery Release

**Release Date:** August 20, 2025  
**Type:** Major UX improvement with auto-extraction

### **New Features**
- **Entity Auto-Discovery** - Base sensor name now auto-extracted from main sensor selection, eliminating manual typing
- **Bed Warming Notifications** - Get notified when print bed reaches target temperature and stays stable for configurable time
- **Enhanced Configuration UI** - Added helpful examples in all input descriptions for new Home Assistant users
- **Streamlined Setup** - Removed manual "Base Sensor Prefix" input field requirement

### **Technical Improvements**
- Smart template extraction safely parses entity IDs (e.g., `sensor.neptune_max_current_print_state` → `neptune_max`)
- Auto-extracted bed temperature sensors (`sensor.{base}_bed_temperature`, `number.{base}_bed_target`)
- Added trigger_variables support for templated triggers
- Maintains full backward compatibility with existing configurations
- Handles standard Moonraker entity naming patterns automatically

### **User Experience**
- Reduced configuration complexity by 20% (one less required input)
- Bed warming feature for pre-heating workflows
- Configurable warm time (1-60 minutes) with all notification channels
- Clear examples guide new users: "(Example: sensor.neptune_max_progress)"
- Auto-extraction prevents common typing errors in base names

### **Testing Status**
- Auto-extraction logic validated
- Backward compatibility confirmed  
- Enhanced descriptions reviewed
- Bed warming notifications implemented
- Real bed warming testing in progress

---

## v1.4.4 - Development (Previous)

**Release Date:** TBD  
**Type:** Critical fixes and code cleanup

### **Critical Fixes**
- **Fixed Alexa time comparison logic** - Replaced broken string comparison with proper hour/minute math to handle midnight crossover correctly. No more 3am wake-ups when quiet hours span overnight (e.g., 22:00 → 09:00).
- **Removed orphaned error trigger reference** - Fixed script condition that referenced non-existent `trigger.id: error`, eliminating automation errors.

### **Configuration Updates**
- **Updated Alexa service default** - Changed from generic group to `notify.alexa_media_shawn_s_echo` for testing
- **Disabled mobile notifications by default** - Set to false for faster testing without mobile app setup

### **Completed Fixes**
- Simplified progress template logic (removed unnecessary regex processing)
- Added entity validation for script hooks
- Fixed completion detection (state name mismatch resolved)

---

## v1.4.3 - Previous Release

**Release Date:** August 2025  
**Type:** Feature release

### Features
- Discord notifications with snapshot support
- Alexa voice alerts with time restrictions
- Mobile and persistent notifications
- Script hooks for custom actions
- Progress reporting with fuzzy logic
- Filament runout detection

---

## Development Notes

**Testing Environment:** Home Assistant VM with Neptune Max printer  
**Critical Areas:** Lines 259-298 formatting must not be modified (HA YAML parser sensitivity)  
**Polling Context:** Moonraker integration polls every 30 seconds, affects progress detection logic