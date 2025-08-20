# HAMMER Changelog

## v1.4.4 - Development (In Progress)

**Release Date:** TBD  
**Type:** Critical fixes and code cleanup

### 🔧 **Critical Fixes**
- **Fixed Alexa time comparison logic** - Replaced broken string comparison with proper hour/minute math to handle midnight crossover correctly. No more 3am wake-ups when quiet hours span overnight (e.g., 22:00 → 09:00).
- **Removed orphaned error trigger reference** - Fixed script condition that referenced non-existent `trigger.id: error`, eliminating automation errors.

### 🛠️ **Configuration Updates**
- **Updated Alexa service default** - Changed from generic group to `notify.alexa_media_shawn_s_echo` for testing
- **Disabled mobile notifications by default** - Set to false for faster testing without mobile app setup

### 📋 **Pending Fixes**
- Simplify progress template logic (remove unnecessary regex processing)
- **Update to Home Assistant versioning format** - Change from v1.4.x to YYYY.MM.V format (e.g., 2025.8.1)
- **Create deploy_to_github.py script** - Automate version management and GitHub deployment like HABA/SunPower projects

### 🧪 **Testing Status**
- ✅ Blueprint passes YAML validation
- ✅ Alexa time logic fixed
- ✅ Error trigger reference removed
- ⏳ Real printer testing in progress

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