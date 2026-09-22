# 🎯 Quick Start Guide - AI Client Onboarding System

## For Team Members

### Viewing the Application

The development server is currently running! Simply open your browser and navigate to:

```
http://localhost:5173/
```

You should see a beautiful dark-themed interface with three service options:
- 🌐 **Website Development**
- 🎨 **Branding**
- ⚡ **Business Automation**

### What's Working Now

✅ **Service Selection Screen**
- Click on any service card to select it
- Smooth animations and hover effects
- Responsive design (try resizing your browser)

✅ **Session Management**
- When you select a service, a new onboarding session is created
- Session ID is generated and tracked

### What's Coming Next

🚧 **Question Flow** (Week 2)
- Step-by-step questions based on selected service
- Progress bar showing completion
- Conditional questions that appear based on previous answers

🚧 **Summary Generation** (Week 3)
- Professional summary of all collected information
- Risk identification (timeline, scope, clarity issues)
- Recommended next steps

🚧 **Export & Integration** (Week 4)
- PDF export of onboarding summary
- ClickUp project creation
- Email notifications to team

---

## For Developers

### Project Structure

```
client-onboarding/
├── src/
│   ├── components/          # React components
│   │   ├── ServiceSelector.tsx
│   │   └── ServiceSelector.css
│   ├── data/               # Question flow configurations
│   │   ├── websiteFlow.ts      # 25+ website questions
│   │   ├── brandingFlow.ts     # 20+ branding questions
│   │   ├── automationFlow.ts   # 25+ automation questions
│   │   └── index.ts
│   ├── types/              # TypeScript definitions
│   │   └── index.ts
│   ├── App.tsx             # Main app component
│   ├── App.css
│   ├── index.css           # Design system
│   └── main.tsx
```

### Key Files to Understand

1. **`src/index.css`** - Complete design system
   - CSS variables for colors, spacing, typography
   - Reusable component styles (buttons, inputs, cards)
   - Animations and transitions

2. **`src/types/index.ts`** - All TypeScript types
   - `ServiceType`, `Question`, `QuestionSection`
   - `OnboardingSession`, `Answer`
   - `OnboardingSummary`, `IdentifiedRisk`

3. **`src/data/websiteFlow.ts`** - Example question flow
   - Shows how questions are structured
   - Demonstrates conditional logic
   - Organized into sections

### Adding/Editing Questions

Questions are defined in the `src/data/` folder. Here's the structure:

```typescript
{
  id: 'unique-question-id',
  type: 'text' | 'textarea' | 'select' | 'multiselect' | 'radio' | 'checkbox',
  label: 'Question text shown to user',
  placeholder: 'Placeholder text (optional)',
  helpText: 'Additional guidance (optional)',
  required: true | false,
  options: ['Option 1', 'Option 2'], // For select/radio/checkbox
  conditionalOn: {  // Optional: show only if condition met
    questionId: 'previous-question-id',
    value: 'specific-answer'
  }
}
```

### Running Commands

```bash
# Start dev server (if not running)
cd client-onboarding
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Type checking
npx tsc --noEmit
```

### Making Changes

1. Edit files in `src/`
2. Save the file
3. Browser will automatically refresh (Hot Module Replacement)
4. Check browser console for any errors

### Design System Usage

The design system is in `src/index.css`. Use these classes in your components:

**Layout:**
- `.container` - Max-width container with padding
- `.container-sm` - Smaller max-width (800px)
- `.container-xs` - Extra small (600px)

**Components:**
- `.glass-card` - Glassmorphism card effect
- `.btn` - Base button style
- `.btn-primary` - Primary action button
- `.btn-secondary` - Secondary button
- `.btn-outline` - Outlined button
- `.input` - Text input / textarea
- `.label` - Form label

**Utilities:**
- `.text-center` - Center text
- `.text-gradient` - Gradient text effect
- `.fade-in` - Fade in animation
- `.slide-in` - Slide in animation

**CSS Variables:**
```css
var(--color-primary)
var(--color-text-primary)
var(--spacing-md)
var(--radius-lg)
var(--transition-base)
```

---

## Testing the Application

### Manual Testing Checklist

✅ **Service Selection**
- [ ] All three service cards are visible
- [ ] Hover effects work smoothly
- [ ] Clicking a card selects the service
- [ ] Mobile responsive (test on small screen)

🚧 **Question Flow** (Coming in Week 2)
- [ ] Questions display correctly
- [ ] Can navigate between sections
- [ ] Answers are saved
- [ ] Conditional questions appear/hide correctly
- [ ] Validation works on required fields

🚧 **Summary** (Coming in Week 3)
- [ ] All answers are displayed
- [ ] Risks are identified correctly
- [ ] PDF export works
- [ ] ClickUp integration creates project

---

## Need Help?

- **Questions about the code?** Check the TypeScript types in `src/types/index.ts`
- **Want to change styling?** Edit `src/index.css` (design system)
- **Need to add/edit questions?** Modify files in `src/data/`
- **Found a bug?** Document it and share with the team

---

## Current Status

**Phase**: Foundation Complete ✅  
**Next Milestone**: Question Rendering Engine (Week 2)  
**Timeline**: On track for 4-6 week MVP  
**Dev Server**: Running at http://localhost:5173/

---

**Last Updated**: January 25, 2026  
**Version**: 0.1.0 (MVP in progress)
