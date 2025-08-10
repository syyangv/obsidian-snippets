# Task Status Customization - Testing Guide

Comprehensive testing guide for the task status customization snippet to ensure compatibility across all modes, themes, and plugins.

## 🧪 **Test Categories**

### **1. Basic Functionality Testing**

#### **Test Cases - Status Rendering**
```markdown
## Basic Task Status Test

- [ ] To Do - Should show gray circle outline (◯)
- [/] In Progress - Should show amber half-filled circle (◐)  
- [x] Done - Should show green checkmark (✓)
- [>] Migrated - Should show blue right arrow (→)
- [?] Planned - Should show violet diamond outline (◇)
- [!] Blocked - Should show red warning triangle (⚠)
- [-] Cancelled - Should show gray X mark (✕)
```

**Expected Results**:
- ✅ Each status displays unique icon and color
- ✅ Icons are centered and properly sized
- ✅ Colors match semantic meanings
- ✅ Hover effects work smoothly

### **2. Tasks Plugin Compatibility Testing**

#### **Test Cases - Plugin Integration**
```markdown
## Tasks Plugin Query Test

```tasks
not done
```

```tasks  
done
```

```tasks
path includes Projects
```
```

**Expected Results**:
- ✅ Query results show custom icons
- ✅ Consistent styling in query blocks
- ✅ Plugin metadata displays correctly
- ✅ No CSS conflicts with plugin styling

### **3. Dual-Mode Testing**

#### **Reading View Test**
1. Switch to Reading view
2. Verify all 7 task statuses render correctly
3. Check icon alignment and spacing
4. Test hover interactions

#### **Live Preview Test**  
1. Switch to Live Preview mode
2. Edit tasks and change statuses
3. Verify real-time status updates
4. Check editor cursor positioning

#### **Source Mode Test**
1. Switch to Source mode (if available)
2. Verify checkboxes still display correctly
3. Check that editing doesn't break styling

**Expected Results**:
- ✅ Consistent appearance across all modes
- ✅ Smooth transitions when switching modes
- ✅ No layout breaks or misalignments

### **4. Theme Compatibility Testing**

#### **Light/Dark Theme Test**
1. Switch to light theme
2. Verify all colors have proper contrast
3. Switch to dark theme  
4. Check color adjustments work correctly

#### **Community Theme Test**
Test with popular themes:
- Minimal Theme
- California Coast
- Blue Topaz
- Things Theme
- Nord Theme

**Expected Results**:
- ✅ Colors adapt to theme context
- ✅ Icons remain visible and clear
- ✅ No visual conflicts with theme styling

### **5. Accessibility Testing**

#### **High Contrast Test**
1. Enable high contrast mode in system settings
2. Verify enhanced borders and text shadows
3. Check readability of all status icons

#### **Reduced Motion Test**
1. Enable reduced motion in system settings
2. Verify animations are disabled/minimized
3. Check basic functionality still works

#### **Screen Reader Test**
1. Use screen reader (VoiceOver/NVDA/JAWS)
2. Navigate through task list
3. Verify status announcements are clear

**Expected Results**:
- ✅ WCAG 2.1 AA compliance maintained
- ✅ Status information communicated to screen readers
- ✅ High contrast support functional

### **6. Performance Testing**

#### **Large Task List Test**
Create document with 100+ tasks:
```markdown
- [ ] Task 1
- [/] Task 2  
- [x] Task 3
- [>] Task 4
- [?] Task 5
- [!] Task 6
- [-] Task 7
... (repeat pattern)
```

**Performance Metrics**:
- ✅ Smooth scrolling maintained
- ✅ No lag when hovering over tasks
- ✅ Fast status switching in edit mode
- ✅ No memory leaks with CSS animations

### **7. Integration Testing**

#### **With Calendar System**
1. Test task statuses in calendar views
2. Verify color consistency with calendar themes
3. Check no z-index conflicts

#### **With Meta-Bind System**
1. Test task statuses with Meta-Bind toggles
2. Verify no CSS selector conflicts
3. Check namespace isolation works

#### **With Other Plugins**
Test compatibility with:
- Kanban plugin
- Projects plugin  
- Dataview plugin
- Day Planner plugin

**Expected Results**:
- ✅ No CSS conflicts with other systems
- ✅ Proper z-index layering maintained
- ✅ Performance not degraded by interactions

## 🔍 **Manual Testing Procedures**

### **Step 1: Enable Snippet**
1. Go to Settings > Appearance > CSS snippets
2. Enable `task-status-customization.css`
3. Restart Obsidian if needed

### **Step 2: Create Test Document**
```markdown
# Task Status Test Document

## All Status Types
- [ ] Regular todo item
- [/] Currently in progress
- [x] Completed task
- [>] Migrated elsewhere  
- [?] Planned for future
- [!] Blocked by dependency
- [-] Cancelled/abandoned

## With Text Content
- [ ] This is a regular todo with longer text content to test wrapping
- [/] **Bold text** and *italic text* should work normally
- [x] Links should work: [[Some Note]]
- [!] `Code snippets` should display correctly

## Nested Tasks
- [ ] Parent task
  - [ ] Nested child task
  - [x] Completed child
    - [/] Deep nested task

## Mixed with Other Elements
Text before tasks

- [ ] Todo after text
- [x] Completed task

More text content

## Tasks Plugin Compatibility
If Tasks plugin is installed, test:

```tasks
not done
path includes Test
```
```

### **Step 3: Visual Inspection Checklist**

#### **Icon Appearance**
- [ ] To Do: Gray circle outline (◯)
- [ ] In Progress: Amber half-circle (◐)  
- [ ] Done: Green checkmark (✓)
- [ ] Migrated: Blue arrow (→)
- [ ] Planned: Violet diamond (◇)
- [ ] Blocked: Red warning (⚠)
- [ ] Cancelled: Gray X (✕)

#### **Color Accuracy**
- [ ] Gray tones for neutral states (Todo, Cancelled)
- [ ] Amber/yellow for active state (In Progress)
- [ ] Green for success state (Done)
- [ ] Blue for movement state (Migrated)
- [ ] Violet for planning state (Planned)
- [ ] Red for attention state (Blocked)

#### **Layout & Spacing**
- [ ] Icons centered in checkboxes
- [ ] Consistent spacing between checkbox and text
- [ ] Proper alignment in nested lists
- [ ] No overlap with other UI elements

#### **Interactions**
- [ ] Hover effects work smoothly
- [ ] Focus indicators visible with keyboard navigation
- [ ] Click areas are appropriate size
- [ ] Status changes work in edit mode

### **Step 4: Cross-Mode Testing**

#### **Reading View**
1. Switch to Reading view
2. Run visual inspection checklist
3. Test all interactive elements

#### **Live Preview**  
1. Switch to Live Preview
2. Edit some task statuses
3. Verify changes appear immediately
4. Check cursor positioning after edits

#### **Source Mode**
1. Switch to Source mode (if available)
2. Verify checkboxes display
3. Test editing doesn't break functionality

## 🚨 **Known Issues & Solutions**

### **Common Issues**

#### **Icons Not Showing**
**Cause**: Font rendering issues or CSS loading problems
**Solution**: 
1. Refresh Obsidian (Ctrl/Cmd + R)
2. Disable and re-enable the CSS snippet
3. Check browser/system font support

#### **Colors Not Matching Theme**
**Cause**: Theme override conflicts
**Solution**:
1. Check theme-specific CSS variables
2. Verify snippet loads after theme
3. Use browser DevTools to debug CSS cascade

#### **Tasks Plugin Conflicts**
**Cause**: CSS selector specificity conflicts
**Solution**:
1. Check plugin CSS load order
2. Verify Tasks plugin version compatibility
3. Use more specific selectors if needed

#### **Performance Issues**
**Cause**: Too many CSS animations or complex selectors
**Solution**:
1. Enable reduced motion in accessibility settings
2. Check for CSS containment support
3. Profile CSS performance in DevTools

### **Debugging Commands**

#### **Browser DevTools**
```javascript
// Check applied styles
getComputedStyle(document.querySelector('input[data-task]'));

// Find CSS rule conflicts  
document.querySelector('input[data-task]').matches(':hover');

// Performance profiling
console.time('task-render');
// ... perform actions
console.timeEnd('task-render');
```

#### **CSS Debug Mode**
Add to snippet temporarily:
```css
.debug-tasks input[type="checkbox"][data-task] {
  outline: 2px solid red !important;
}
```

## ✅ **Test Completion Checklist**

- [ ] All 7 task statuses render correctly
- [ ] Icons display with proper colors and alignment
- [ ] Hover and focus effects work smoothly  
- [ ] Tasks plugin integration functional (if installed)
- [ ] Consistent appearance across Reading/Live Preview modes
- [ ] Theme compatibility verified (light/dark minimum)
- [ ] Accessibility features working (high contrast, reduced motion)
- [ ] Performance acceptable with large task lists (100+ tasks)
- [ ] No conflicts with existing CSS snippets
- [ ] Screen reader compatibility verified
- [ ] Mobile/responsive design working
- [ ] Cross-browser compatibility tested

## 📊 **Test Results Template**

```markdown
# Task Status Testing Results

**Date**: [Date]
**Obsidian Version**: [Version]
**Platform**: [Windows/Mac/Linux]
**Theme**: [Theme Name]
**Tasks Plugin Version**: [Version if installed]

## Test Results

### Basic Functionality: ✅/❌
- Status icons: ✅/❌
- Color accuracy: ✅/❌  
- Hover effects: ✅/❌
- Click interactions: ✅/❌

### Mode Compatibility: ✅/❌
- Reading view: ✅/❌
- Live preview: ✅/❌
- Source mode: ✅/❌

### Plugin Integration: ✅/❌
- Tasks plugin: ✅/❌/N/A
- Other plugins: ✅/❌

### Accessibility: ✅/❌
- High contrast: ✅/❌
- Reduced motion: ✅/❌
- Screen reader: ✅/❌

### Performance: ✅/❌
- Large lists: ✅/❌
- Smooth animations: ✅/❌
- Memory usage: ✅/❌

## Issues Found
[List any issues with reproduction steps]

## Notes
[Additional observations]
```

This comprehensive testing guide ensures the task status customization snippet works reliably across all use cases and maintains enterprise-grade quality standards.