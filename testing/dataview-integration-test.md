# Dataview Integration Test - Task Status Customization

Test document to validate that custom task status icons work correctly in Dataview queries.

## 🧪 **Test Setup**

### Prerequisites
1. **Task Status Customization** snippet enabled
2. **Dataview plugin** installed and enabled
3. Test files with various task statuses created

### Test Files Required
Create these test files in your vault:

#### `Tasks-Test-1.md`
```markdown
# Project Alpha Tasks

- [ ] Review project requirements
- [/] Implement authentication system
- [x] Set up development environment
- [>] Move legacy data to new system
- [?] Plan Q4 features
- [!] Blocked by API team
- [-] Cancelled feature request
```

#### `Tasks-Test-2.md` 
```markdown
# Daily Tasks

- [ ] Check emails
- [/] Write documentation
- [x] Team standup meeting
- [>] Transfer notes to Notion
- [?] Schedule next sprint
- [!] Waiting for design approval
- [-] Old meeting cancelled
```

## 🔍 **Dataview Query Tests**

### Test 1: Basic Task Query
```dataview
TASK
FROM "Tasks-Test"
```

**Expected Result**: All task statuses should show their custom icons and colors

### Test 2: Filtered by Status - Not Done
```dataview
TASK
FROM "Tasks-Test"
WHERE !completed
```

**Expected Result**: Only incomplete tasks ([ ], [/], [>], [?], [!], [-]) with proper icons

### Test 3: Filtered by Status - Completed Only
```dataview
TASK  
FROM "Tasks-Test"
WHERE completed
```

**Expected Result**: Only completed tasks ([x]) with green checkmark

### Test 4: Filtered by File
```dataview
TASK
FROM "Tasks-Test-1"
```

**Expected Result**: Only tasks from Tasks-Test-1.md with proper styling

### Test 5: Text Contains Filter
```dataview
TASK
FROM "Tasks-Test"
WHERE contains(text, "system")
```

**Expected Result**: Tasks containing "system" with proper icon styling

### Test 6: Combined with Table View
```dataview
TABLE 
  file.name as File,
  length(file.tasks) as "Total Tasks",
  length(filter(file.tasks, (t) => t.completed)) as "Completed"
FROM "Tasks-Test"
```

**Expected Result**: Table should display alongside task icons

## ✅ **Visual Validation Checklist**

### Icons Display Correctly
- [ ] **To Do** `[ ]` → Gray circle outline (◯)
- [ ] **In Progress** `[/]` → Amber half-filled circle (◐)
- [ ] **Done** `[x]` → Green checkmark (✓)
- [ ] **Migrated** `[>]` → Blue right arrow (→)
- [ ] **Planned** `[?]` → Violet diamond outline (◇)
- [ ] **Blocked** `[!]` → Red warning triangle (⚠)
- [ ] **Cancelled** `[-]` → Gray X mark (✕)

### Colors Match Semantic System
- [ ] Gray tones for neutral states (To Do, Cancelled)
- [ ] Amber/yellow for active state (In Progress)
- [ ] Green for success state (Done)
- [ ] Blue for movement state (Migrated)
- [ ] Violet for planning state (Planned)
- [ ] Red for attention state (Blocked)

### Layout and Spacing
- [ ] Icons are centered in checkboxes
- [ ] Proper spacing between checkbox and task text
- [ ] No overlap with Dataview query metadata
- [ ] Consistent alignment across all query results

### Interactive Elements
- [ ] Hover effects work on checkboxes in query results
- [ ] Focus indicators visible with keyboard navigation
- [ ] Clicking checkboxes toggles status (if editable)
- [ ] No layout shifts when hovering

## 🐛 **Common Issues and Solutions**

### Icons Not Showing in Dataview
**Symptoms**: Dataview tasks show default checkboxes without custom icons

**Possible Causes**:
1. CSS specificity conflict with Dataview plugin
2. Dataview renders tasks before CSS loads
3. Different HTML structure than expected

**Solutions**:
1. **Refresh the page** - Press `Ctrl+R` (Windows) or `Cmd+R` (Mac)
2. **Disable and re-enable** the task status CSS snippet
3. **Check CSS specificity** using browser DevTools:
   ```javascript
   // In browser console
   document.querySelectorAll('.dataview input[type="checkbox"]')
   ```

### Inconsistent Styling Between Views
**Symptoms**: Icons work in regular task lists but not in Dataview queries

**Solution**: Verify Dataview-specific CSS selectors are loaded
```css
/* Check these selectors are present in the CSS */
.dataview input[type="checkbox"][data-task="/"]
.block-language-dataview input[type="checkbox"][data-task="/"]
```

### Performance Issues with Large Queries
**Symptoms**: Slow rendering or lag when Dataview queries contain many tasks

**Solution**: 
1. Use CSS containment (already implemented)
2. Limit query scope with specific FROM clauses
3. Consider using Dataview's query limits

## 🔧 **Debug Mode Testing**

### Enable Debug Mode
Add this CSS temporarily to visualize what's being targeted:

```css
.dataview input[type="checkbox"] {
  outline: 2px solid red !important;
}

.block-language-dataview input[type="checkbox"] {
  outline: 2px solid blue !important;
}
```

### Browser DevTools Inspection
1. Right-click on a task checkbox in Dataview results
2. Select "Inspect Element"
3. Verify these CSS classes are present:
   - `.dataview`
   - `.block-language-dataview`
   - `input[type="checkbox"][data-task="X"]`

## 📊 **Performance Testing**

### Large Query Test
Create a query that returns 50+ tasks:
```dataview
TASK
FROM ""
LIMIT 50
```

**Performance Expectations**:
- [ ] Smooth scrolling through results
- [ ] No lag when hovering over checkboxes
- [ ] Icons render consistently across all results
- [ ] Page remains responsive

### Memory Usage Test
1. Open browser DevTools → Performance tab
2. Record performance while loading large Dataview queries
3. Check for memory leaks or excessive repaints
4. Verify CSS containment is working

## ✅ **Test Results Template**

```markdown
# Dataview Integration Test Results

**Date**: [Date]
**Obsidian Version**: [Version]
**Dataview Plugin Version**: [Version]
**Theme**: [Theme Name]

## Basic Functionality
- Task icons in queries: ✅/❌
- Color accuracy: ✅/❌
- Hover effects: ✅/❌
- Layout consistency: ✅/❌

## Query Types Tested
- Basic TASK queries: ✅/❌
- Filtered queries: ✅/❌
- Combined with TABLE: ✅/❌
- FROM specific files: ✅/❌

## Performance
- Large queries (50+ tasks): ✅/❌
- Smooth interactions: ✅/❌
- Memory usage acceptable: ✅/❌

## Cross-Mode Compatibility  
- Reading view: ✅/❌
- Live preview: ✅/❌
- Regular task lists: ✅/❌

## Issues Found
[Describe any issues with reproduction steps]

## Additional Notes
[Any observations about integration]
```

## 🚀 **Advanced Dataview Integration Examples**

### Custom Task Dashboard
```dataview
TABLE WITHOUT ID
  "📁 " + file.name as File,
  "📋 " + length(file.tasks) as Total,
  "✅ " + length(filter(file.tasks, (t) => t.status = "x")) as Done,
  "🚧 " + length(filter(file.tasks, (t) => t.status = "/")) as "In Progress",
  "🚫 " + length(filter(file.tasks, (t) => t.status = "!")) as Blocked
FROM "Tasks-Test"
WHERE file.tasks
SORT length(file.tasks) DESC
```

### Task Status Distribution
```dataview
TABLE
  choice(status = " ", "📝 To Do",
    choice(status = "/", "🚧 In Progress", 
      choice(status = "x", "✅ Done",
        choice(status = ">", "📤 Migrated",
          choice(status = "?", "📋 Planned",
            choice(status = "!", "🚫 Blocked",
              choice(status = "-", "❌ Cancelled", "❓ Unknown"))))))) as Status,
  length(rows) as Count
FROM "Tasks-Test"
FLATTEN file.tasks as tasks
WHERE tasks
GROUP BY tasks.status as status
SORT Count DESC
```

This comprehensive test ensures that the task status customization system works perfectly with Dataview queries, maintaining the enterprise-grade quality across all integration points.