# GetKlean Admin — shadcn + Tailwind Design System

**Version:** 1.0
**Source:** `DESIGN-SYSTEM-SPEC.md`
**Stack:** Next.js 15 (App Router) · shadcn/ui · Tailwind CSS v4 · lucide-react · TypeScript

---

## HOW TO USE THIS FILE

| Section | Purpose |
|---|---|
| §2 | Copy into `src/app/globals.css` |
| §3 | Reference for `tailwind.config.ts` (v3) or `@theme` (v4) |
| §4 | `npx shadcn@latest add` commands + usage patterns |
| §5 | Build these custom components not in shadcn |
| §6 | Scaffold the dashboard layout shell |
| §7 | State class quick-reference |
| §8 | Implementation checklist |

---

## 1. TAILWIND TOKEN SYSTEM

### 1.1 Colors

| Tailwind name | Hex | HSL (shadcn format) | Usage |
|---|---|---|---|
| `primary` | `#3886C8` | `208 57% 50%` | Actions, links, focus rings |
| `primary-dark` | `#2863A4` | `213 61% 40%` | Primary hover |
| `primary-light` | `#55A1DA` | `206 63% 60%` | Primary tint surfaces |
| `secondary` | `#FFEB3B` | `54 100% 62%` | Accent, active nav highlight |
| `secondary-dark` | `#FBC02D` | `43 96% 58%` | Secondary hover |
| `secondary-light` | `#FFF176` | `56 100% 73%` | Secondary surface tint |
| `destructive` | `#D32F2F` | `0 63% 50%` | Error, danger actions |
| `warning` | `#EF6C00` | `27 100% 47%` | Warning, For-review badge |
| `success` | `#2E7D32` | `123 46% 34%` | Success, Confirmed badge |
| `info` | `#0288D1` | `201 98% 41%` | Info, In-progress badge |
| `neutral-100` | `#F1F5F9` | `210 40% 96%` | Nav hover bg, sidebar accent, muted surfaces |
| `neutral-300` | `#BDBDBD` | `0 0% 74%` | Borders, disabled, empty stars |
| `neutral-500` | `#757575` | `0 0% 46%` | Muted text, Waived badge |

### 1.2 Typography

| Property | Value |
|---|---|
| Font family | `Montserrat, Helvetica, Arial, sans-serif` |
| `xs` | 12px — captions, timestamps, badge labels |
| `sm` | 14px — body2, inputs, labels, nav items |
| `base` | 16px — body1 |
| `lg` | 18px — h5 |
| `xl` | 20px — h4, page titles |
| `2xl` | 24px — h3 |
| `3xl` | 30px — h2 |
| `4xl` | 36px — h1 |
| `light` | 300 — h3, h4 |
| `normal` | 400 — body |
| `medium` | 500 — buttons, headings, nav items |
| `semibold` | 600 — section headings (`DetailSection`) |
| `bold` | 700 — KPI values |
| Button letter-spacing | `0.46px` |

### 1.3 Spacing

Base: **8px**. Tailwind's default 4px base — use standard Tailwind spacing as-is.

| Token | px | Common usage |
|---|---|---|
| `1` | 4px | Icon padding, tight gaps |
| `2` | 8px | Component internal (sm) |
| `3` | 12px | Button-sm horizontal, input padding |
| `4` | 16px | Standard padding, card gap |
| `5` | 20px | Input margin-bottom |
| `6` | 24px | Section gaps, content padding |
| `8` | 32px | Large gaps, OTP margin |
| `10` | 40px | Section dividers |
| `12` | 48px | Page-level padding |

### 1.4 Border Radius

| Token | px | Tailwind | Usage |
|---|---|---|---|
| `sm` | 4px | `rounded` | Tags, subtle corners |
| `md` | 8px | `rounded-md` | Buttons, inputs, cards — **default** |
| `lg` | 12px | `rounded-lg` | Large cards, panels |
| `xl` | 16px | `rounded-xl` | Modal dialogs |
| `full` | 9999px | `rounded-full` | Pills, avatars, chips |
| OTP | 10px | `rounded-[10px]` | OTP digit boxes only |

> `--radius: 0.5rem` (8px) is the shadcn base radius.

### 1.5 Shadows

| Token | Value | Tailwind |
|---|---|---|
| `sm` | `0 1px 2px 0 rgba(0,0,0,0.05)` | `shadow-sm` |
| `md` | `0 4px 6px -1px rgba(0,0,0,0.10), 0 2px 4px -1px rgba(0,0,0,0.06)` | `shadow-md` |
| `lg` | `0 10px 15px -3px rgba(0,0,0,0.10), 0 4px 6px -2px rgba(0,0,0,0.05)` | `shadow-lg` |
| `xl` | `0 20px 25px -5px rgba(0,0,0,0.10), 0 10px 10px -5px rgba(0,0,0,0.04)` | `shadow-xl` |

---

## 2. CSS VARIABLE SYSTEM

Full `src/app/globals.css`:

```css
@import "tailwindcss";

/* ─── Tailwind v4 @theme ─────────────────────────────────────────── */
@theme {
  /* Font */
  --font-family-sans: Montserrat, Helvetica, Arial, sans-serif;

  /* Brand colors (raw hex — for Tailwind utilities: bg-primary, text-warning, etc.) */
  --color-primary:          #3886C8;
  --color-primary-dark:     #2863A4;
  --color-primary-light:    #55A1DA;
  --color-secondary:        #FFEB3B;
  --color-secondary-dark:   #FBC02D;
  --color-secondary-light:  #FFF176;
  --color-destructive:      #D32F2F;
  --color-warning:          #EF6C00;
  --color-success:          #2E7D32;
  --color-info:             #0288D1;
  --color-neutral-100:      #F1F5F9;
  --color-neutral-300:      #BDBDBD;
  --color-neutral-500:      #757575;

  /* Radius */
  --radius-sm:   0.25rem;
  --radius-md:   0.5rem;
  --radius-lg:   0.75rem;
  --radius-xl:   1rem;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px -1px rgba(0,0,0,0.10), 0 2px 4px -1px rgba(0,0,0,0.06);
  --shadow-lg: 0 10px 15px -3px rgba(0,0,0,0.10), 0 4px 6px -2px rgba(0,0,0,0.05);
  --shadow-xl: 0 20px 25px -5px rgba(0,0,0,0.10), 0 10px 10px -5px rgba(0,0,0,0.04);

  /* Transitions */
  --transition-fast:   150ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-normal: 250ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-slow:   350ms cubic-bezier(0.4, 0, 0.2, 1);

  /* Z-index */
  --z-dropdown:       1000;
  --z-sticky:         1020;
  --z-fixed:          1030;
  --z-modal-backdrop: 1040;
  --z-modal:          1050;
  --z-popover:        1060;
  --z-tooltip:        1070;
}

/* ─── shadcn semantic tokens (HSL — no hsl() wrapper) ───────────── */
:root {
  --background:             0 0% 100%;        /* #FFFFFF */
  --foreground:             0 0% 9%;           /* #171717 */

  --card:                   0 0% 100%;
  --card-foreground:        0 0% 9%;

  --popover:                0 0% 100%;
  --popover-foreground:     0 0% 9%;

  --primary:                208 57% 50%;       /* #3886C8 */
  --primary-foreground:     0 0% 100%;         /* #FFFFFF */

  --secondary:              54 100% 62%;       /* #FFEB3B */
  --secondary-foreground:   0 0% 7%;           /* #121212 */

  --muted:                  0 0% 96%;          /* #F5F5F5 */
  --muted-foreground:       0 0% 46%;          /* #757575 */

  --accent:                 54 100% 62%;       /* #FFEB3B */
  --accent-foreground:      0 0% 7%;

  --destructive:            0 63% 50%;         /* #D32F2F */
  --destructive-foreground: 0 0% 100%;

  --border:                 0 0% 88%;          /* #E0E0E0 ≈ rgba(0,0,0,0.12) */
  --input:                  0 0% 88%;
  --ring:                   208 57% 50%;       /* #3886C8 — focus rings */
  --radius:                 0.5rem;            /* 8px */

  /* GK Layout tokens */
  --gk-topbar-height:   64px;
  --gk-topbar-bg:       #FFFFFF;
  --gk-sidebar-width:   260px;
  --gk-sidebar-bg:      #FFFFFF;
  --gk-footer-bg:       #37474F;
  --gk-content-padding: 24px;
}

/* ─── Dark mode ──────────────────────────────────────────────────── */
[data-theme="dark"],
.dark {
  --background:             0 0% 7%;           /* #121212 */
  --foreground:             0 0% 93%;

  --card:                   0 0% 12%;          /* #1E1E1E */
  --card-foreground:        0 0% 93%;

  --popover:                0 0% 12%;
  --popover-foreground:     0 0% 93%;

  --primary:                207 65% 75%;       /* #95C7E9 dark mode */
  --primary-foreground:     0 0% 7%;

  --secondary:              54 100% 62%;
  --secondary-foreground:   0 0% 7%;

  --muted:                  0 0% 18%;          /* #2D2D2D */
  --muted-foreground:       0 0% 60%;

  --accent:                 54 100% 62%;
  --accent-foreground:      0 0% 7%;

  --destructive:            0 72% 59%;         /* #F44336 */
  --destructive-foreground: 0 0% 100%;

  --border:                 0 0% 25%;
  --input:                  0 0% 25%;
  --ring:                   207 65% 75%;
}

/* ─── Base ───────────────────────────────────────────────────────── */
@layer base {
  * {
    @apply border-border;
  }
  body {
    @apply bg-background text-foreground;
    font-family: var(--font-family-sans);
  }
  /* Interactive transition defaults */
  button, a, input, textarea, select {
    transition-property: color, background-color, border-color, box-shadow, transform;
    transition-duration: 150ms;
    transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  }
}
```

---

## 3. TAILWIND CONFIG (v3 reference)

```ts
// tailwind.config.ts
import type { Config } from 'tailwindcss';

const config: Config = {
  darkMode: ['class', '[data-theme="dark"]'],
  content: [
    './src/pages/**/*.{ts,tsx}',
    './src/components/**/*.{ts,tsx}',
    './src/app/**/*.{ts,tsx}',
  ],
  theme: {
    extend: {
      fontFamily: {
        sans: ['Montserrat', 'Helvetica', 'Arial', 'sans-serif'],
      },
      colors: {
        border:     'hsl(var(--border))',
        input:      'hsl(var(--input))',
        ring:       'hsl(var(--ring))',
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        primary: {
          DEFAULT:    'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
          dark:       '#2863A4',
          light:      '#55A1DA',
        },
        secondary: {
          DEFAULT:    'hsl(var(--secondary))',
          foreground: 'hsl(var(--secondary-foreground))',
          dark:       '#FBC02D',
          light:      '#FFF176',
        },
        destructive: {
          DEFAULT:    'hsl(var(--destructive))',
          foreground: 'hsl(var(--destructive-foreground))',
        },
        muted: {
          DEFAULT:    'hsl(var(--muted))',
          foreground: 'hsl(var(--muted-foreground))',
        },
        accent: {
          DEFAULT:    'hsl(var(--accent))',
          foreground: 'hsl(var(--accent-foreground))',
        },
        card: {
          DEFAULT:    'hsl(var(--card))',
          foreground: 'hsl(var(--card-foreground))',
        },
        popover: {
          DEFAULT:    'hsl(var(--popover))',
          foreground: 'hsl(var(--popover-foreground))',
        },
        // GK semantic extras
        warning:        '#EF6C00',
        success:        '#2E7D32',
        info:           '#0288D1',
        'neutral-100':  '#F1F5F9',
        'neutral-300':  '#BDBDBD',
        'neutral-500':  '#757575',
      },
      borderRadius: {
        sm:    '4px',
        DEFAULT: '8px',
        md:    '8px',
        lg:    '12px',
        xl:    '16px',
        '2xl': '20px',
        full:  '9999px',
      },
      boxShadow: {
        sm: '0 1px 2px 0 rgba(0,0,0,0.05)',
        md: '0 4px 6px -1px rgba(0,0,0,0.10), 0 2px 4px -1px rgba(0,0,0,0.06)',
        lg: '0 10px 15px -3px rgba(0,0,0,0.10), 0 4px 6px -2px rgba(0,0,0,0.05)',
        xl: '0 20px 25px -5px rgba(0,0,0,0.10), 0 10px 10px -5px rgba(0,0,0,0.04)',
      },
      transitionDuration: {
        fast:   '150ms',
        normal: '250ms',
        slow:   '350ms',
      },
    },
  },
  plugins: [require('tailwindcss-animate')],
};

export default config;
```

---

## 4. SHADCN COMPONENT MAPPING

### Install all required components

```bash
# Init shadcn
npx shadcn@latest init

# Install all components used in GKAdmin
npx shadcn@latest add \
  button input label form \
  card alert \
  dialog \
  table \
  tabs accordion \
  select checkbox radio-group textarea \
  tooltip popover dropdown-menu \
  badge pagination separator avatar skeleton \
  sheet
```

---

### 4.1 Button

**MUI:** `contained` | `outlined` | `text`
**shadcn:** `Button` extended with `cva`

Extend `src/components/ui/button.tsx` — add these variants:

```ts
const buttonVariants = cva(
  // Base — applies to all variants
  'inline-flex items-center justify-center gap-2 font-medium tracking-[0.46px] rounded-md transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring/30 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:bg-black/12 disabled:text-black/26 disabled:shadow-none',
  {
    variants: {
      variant: {
        // Primary filled
        contained:            'bg-primary text-primary-foreground shadow-sm hover:bg-primary-dark hover:shadow-md',
        // Border only
        outlined:             'border border-primary text-primary bg-transparent hover:bg-primary/8',
        // No border, no bg
        text:                 'text-primary bg-transparent hover:bg-primary/8',
        // Danger filled
        destructive:          'bg-destructive text-destructive-foreground shadow-sm hover:bg-destructive/90',
        // Danger outlined
        'destructive-outlined':'border border-destructive text-destructive bg-transparent hover:bg-destructive/8',
        // Yellow / secondary
        secondary:            'bg-secondary text-secondary-foreground hover:bg-secondary-dark',
        // Ghost (for icon buttons, topbar controls)
        ghost:                'bg-transparent hover:bg-black/4 text-foreground',
        // White-on-primary (topbar)
        'ghost-white':        'bg-transparent hover:bg-white/10 text-white',
      },
      size: {
        sm:   'h-8 px-3 text-sm',
        md:   'h-10 px-4 text-sm',
        lg:   'h-12 px-[22px] text-base',
        icon: 'h-9 w-9 p-0',
      },
    },
    defaultVariants: { variant: 'contained', size: 'md' },
  }
);
```

**Loading state pattern:**
```tsx
<Button disabled={loading}>
  {loading
    ? <Loader2 size={20} strokeWidth={1.5} className="animate-spin" />
    : children}
</Button>
```

**State summary:**

| State | Class |
|---|---|
| Hover (contained) | `hover:bg-primary-dark hover:shadow-md` |
| Hover (outlined/text) | `hover:bg-primary/8` |
| Focus | `focus-visible:ring-2 focus-visible:ring-ring/30` |
| Disabled | `disabled:bg-black/12 disabled:text-black/26` |
| Loading | Show `<Loader2 className="animate-spin" />`, hide label |

---

### 4.2 Input (TextField)

**MUI:** `outlined` | `filled` | `standard`
**shadcn:** `Input` + `Label` + `FormMessage`

Override classes in `src/components/ui/input.tsx`:

```tsx
className={cn(
  'flex h-10 w-full rounded-md border border-input bg-background px-3 py-2 text-sm',
  'ring-offset-background placeholder:text-muted-foreground',
  'hover:border-primary',
  'focus-visible:outline-none focus-visible:border-primary focus-visible:ring-2 focus-visible:ring-ring/30',
  'disabled:cursor-not-allowed disabled:opacity-50',
  'aria-[invalid=true]:border-destructive aria-[invalid=true]:focus-visible:ring-destructive/30',
  className
)}
```

**With form validation (react-hook-form + zod):**
```tsx
<FormField control={form.control} name="bookingDate" render={({ field }) => (
  <FormItem>
    <FormLabel>Booking date</FormLabel>
    <FormControl>
      <Input type="date" {...field} />
    </FormControl>
    <FormMessage />   {/* renders zod error automatically */}
  </FormItem>
)} />
```

**State summary:**

| State | Class |
|---|---|
| Default border | `border-input` |
| Hover | `hover:border-primary` |
| Focus | `focus-visible:border-primary focus-visible:ring-2 focus-visible:ring-ring/30` |
| Error | `aria-[invalid=true]:border-destructive` |
| Disabled | `disabled:opacity-50 disabled:cursor-not-allowed` |

---

### 4.3 Card

**MUI:** `elevated` | `outlined` | `filled`
**shadcn:** `Card` + custom `variant` via `cva`

```ts
const cardVariants = cva('rounded-lg', {
  variants: {
    variant: {
      elevated: 'bg-card text-card-foreground shadow-sm hover:shadow-md hover:-translate-y-0.5 transition-all duration-300',
      outlined: 'bg-card text-card-foreground border border-border hover:border-primary hover:-translate-y-0.5 transition-all duration-300',
      filled:   'bg-muted text-card-foreground hover:bg-card hover:-translate-y-0.5 transition-all duration-300',
    },
    padding: {
      sm: 'p-2',
      md: 'p-4',
      lg: 'p-6',
    },
  },
  defaultVariants: { variant: 'elevated', padding: 'md' },
});
```

---

### 4.4 Alert (AlertMessage)

**MUI:** Custom `AlertMessage`
**shadcn:** `Alert` + `AlertDescription` with style override

```tsx
// src/components/ui/alert-message.tsx
const ALERT_COLORS: Record<AlertType, string> = {
  success: '#2E7D32',
  error:   '#D32F2F',
  info:    '#3886C8',
  warning: '#EF6C00',
};

export function AlertMessage({ type = 'info', message }: AlertMessageProps) {
  const color = ALERT_COLORS[type];
  return (
    <Alert
      className="rounded-md border px-4 py-3 text-sm font-medium"
      style={{ background: color + '20', borderColor: color + '40', color }}
    >
      <AlertDescription>{message}</AlertDescription>
    </Alert>
  );
}
```

---

### 4.5 Dialog (Modal / Confirmation)

**MUI:** `Dialog` / `DialogActions`
**shadcn:** `Dialog` + `DialogContent` + `DialogHeader` + `DialogFooter`

**Confirmation pattern:**
```tsx
<Dialog open={open} onOpenChange={setOpen}>
  <DialogContent className="max-w-sm rounded-xl">
    <DialogHeader>
      <DialogTitle>Update service details</DialogTitle>
      <DialogDescription>
        Do you want to save the changes to the service details of {bookingCode}?
      </DialogDescription>
    </DialogHeader>
    <DialogFooter className="gap-2 sm:gap-2">
      <Button variant="outlined" size="md" onClick={() => setOpen(false)}>Cancel</Button>
      <Button variant="contained" size="md" onClick={onConfirm}>Confirm</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

**Success / close-only pattern:**
```tsx
<DialogContent className="max-w-sm rounded-xl text-center">
  <DialogHeader>
    <DialogTitle className="text-success">Updated service details</DialogTitle>
    <DialogDescription>
      Service details for {bookingCode} have been successfully updated.
    </DialogDescription>
  </DialogHeader>
  <DialogFooter className="justify-center">
    <Button variant="contained" size="md" onClick={() => setOpen(false)}>Close</Button>
  </DialogFooter>
</DialogContent>
```

---

### 4.6 Table (DataTable — Bookings)

**MUI:** `MuiTable` / `MuiDataGrid`
**shadcn:** `Table` + `@tanstack/react-table`

```bash
npx shadcn@latest add table
npm install @tanstack/react-table
```

```tsx
// Structure
<div className="rounded-lg border border-border overflow-hidden">
  <Table>
    <TableHeader className="bg-muted">
      <TableRow className="hover:bg-muted">
        <TableHead className="font-medium text-foreground">Booking code</TableHead>
        <TableHead className="font-medium text-foreground">Client name</TableHead>
        <TableHead className="font-medium text-foreground">Address</TableHead>
        <TableHead className="font-medium text-foreground">Service type</TableHead>
        <TableHead className="font-medium text-foreground">Booking date</TableHead>
        <TableHead className="font-medium text-foreground">Status</TableHead>
        <TableHead className="w-12" />
      </TableRow>
    </TableHeader>
    <TableBody>
      {rows.map((row) => (
        <TableRow
          key={row.id}
          className="cursor-pointer hover:bg-muted/50 transition-colors"
          onClick={() => router.push(`/bookings/${row.id}`)}
        >
          <TableCell className="font-mono text-sm font-medium">{row.code}</TableCell>
          <TableCell>{row.clientName}</TableCell>
          <TableCell className="text-muted-foreground">{row.address}</TableCell>
          <TableCell>{row.serviceType}</TableCell>
          <TableCell>{row.bookingDate}</TableCell>
          <TableCell><StatusChip status={row.status} /></TableCell>
          <TableCell>
            <Button variant="ghost" size="icon" onClick={(e) => { e.stopPropagation(); onEdit(row); }}>
              <Pencil size={16} strokeWidth={1.5} />
            </Button>
          </TableCell>
        </TableRow>
      ))}
      {rows.length === 0 && (
        <TableRow>
          <TableCell colSpan={7}>
            <EmptyState variant="no-results" heading="No bookings found" subtext="Try adjusting your filters." />
          </TableCell>
        </TableRow>
      )}
    </TableBody>
  </Table>
</div>
```

---

### 4.7 Tabs (Booking Detail)

**MUI:** `MuiTabs` with underline indicator
**shadcn:** `Tabs` — override to underline style (not pill)

```tsx
<Tabs defaultValue="booking-summary">
  <TabsList className="h-auto w-full justify-start rounded-none border-b border-border bg-transparent p-0 gap-0">
    {['booking-summary', 'client-information', 'assigned-housemaid'].map((tab) => (
      <TabsTrigger
        key={tab}
        value={tab}
        className="rounded-none border-b-2 border-transparent px-4 pb-3 pt-2 text-sm font-medium text-muted-foreground
                   data-[state=active]:border-primary data-[state=active]:text-primary data-[state=active]:shadow-none
                   hover:text-foreground transition-colors"
      >
        {tab === 'booking-summary'    ? 'Booking summary'    :
         tab === 'client-information' ? 'Client information' : 'Assigned Housemaid'}
      </TabsTrigger>
    ))}
  </TabsList>
  <TabsContent value="booking-summary" className="pt-6">...</TabsContent>
  <TabsContent value="client-information" className="pt-6">...</TabsContent>
  <TabsContent value="assigned-housemaid" className="pt-6">...</TabsContent>
</Tabs>
```

---

### 4.8 Accordion (Transportation Summary)

**MUI:** `MuiAccordion`
**shadcn:** `Accordion`

```tsx
<Accordion type="single" collapsible defaultValue="">
  <AccordionItem value="transport" className="border rounded-md px-4">
    <AccordionTrigger className="text-sm font-medium text-foreground hover:no-underline py-3">
      Heading to the Client
    </AccordionTrigger>
    <AccordionContent className="text-sm">
      <p className="text-muted-foreground mb-2">Mode of transportation</p>
      <p className="mb-4">Commute</p>
      {/* Fare breakdown */}
      <div className="space-y-1 border-t pt-3">
        <div className="flex justify-between"><span className="text-muted-foreground">Tricycle</span><span>Php 40.00</span></div>
        <div className="flex justify-between"><span className="text-muted-foreground">Jeep</span><span>Php 15.00</span></div>
        <div className="flex justify-between font-semibold border-t pt-1 mt-1"><span>Total fare</span><span>Php 55.00</span></div>
      </div>
    </AccordionContent>
  </AccordionItem>
</Accordion>
```

---

### 4.9 Select & Multi-Select

**Single select:**
```tsx
<Select onValueChange={setValue}>
  <SelectTrigger className="w-full hover:border-primary focus:border-primary focus:ring-2 focus:ring-ring/30">
    <SelectValue placeholder="Select status" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="for-review">For review</SelectItem>
    <SelectItem value="confirmed">Confirmed</SelectItem>
    <SelectItem value="in-transit">In transit</SelectItem>
    <SelectItem value="completed">Completed</SelectItem>
    <SelectItem value="cancelled">Cancelled</SelectItem>
  </SelectContent>
</Select>
```

**Multi-select (status filter — Popover + Checkbox):**
```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button variant="outlined" size="md" className="justify-between gap-2">
      Status {selected.length > 0 && <Badge>{selected.length}</Badge>}
      <ChevronDown size={16} />
    </Button>
  </PopoverTrigger>
  <PopoverContent className="w-56 p-2">
    {STATUS_OPTIONS.map((opt) => (
      <label key={opt.value} className="flex items-center gap-2 px-2 py-1.5 rounded hover:bg-muted cursor-pointer text-sm">
        <Checkbox
          checked={selected.includes(opt.value)}
          onCheckedChange={(checked) => toggleStatus(opt.value, !!checked)}
        />
        {opt.label}
      </label>
    ))}
  </PopoverContent>
</Popover>
```

---

### 4.10 Checkbox (Service selection)

```tsx
// 2-column service checkbox grid
<div className="grid grid-cols-2 gap-x-6 gap-y-3">
  {SERVICES.map((service) => (
    <label key={service.id} className="flex items-center gap-2 cursor-pointer text-sm">
      <Checkbox
        id={service.id}
        checked={selected.includes(service.id)}
        onCheckedChange={(v) => onToggle(service.id, !!v)}
        className="data-[state=checked]:bg-primary data-[state=checked]:border-primary"
      />
      <span>{service.label}</span>
    </label>
  ))}
</div>
```

---

### 4.11 RadioGroup (Service duration)

```tsx
<RadioGroup defaultValue="whole-day" className="flex gap-6">
  <div className="flex items-center gap-2">
    <RadioGroupItem value="half-day" id="half-day"
      className="border-primary text-primary" />
    <Label htmlFor="half-day" className="cursor-pointer">Half day (4 hours)</Label>
  </div>
  <div className="flex items-center gap-2">
    <RadioGroupItem value="whole-day" id="whole-day"
      className="border-primary text-primary" />
    <Label htmlFor="whole-day" className="cursor-pointer">Whole day (8 hours)</Label>
  </div>
</RadioGroup>
```

---

### 4.12 Textarea

```tsx
<div className="space-y-1.5">
  <Label htmlFor="reason">Reason for dissatisfaction</Label>
  <Textarea
    id="reason"
    placeholder="Reason for dissatisfaction"
    className="min-h-[80px] resize-none hover:border-primary
               focus-visible:border-primary focus-visible:ring-2 focus-visible:ring-ring/30
               aria-[invalid=true]:border-destructive"
    aria-invalid={!!error}
  />
  {error && <p className="text-xs text-destructive">{error}</p>}
</div>
```

---

### 4.13 Tooltip (Icon button labels)

```tsx
<TooltipProvider>
  <Tooltip>
    <TooltipTrigger asChild>
      <Button variant="ghost" size="icon">
        <Pencil size={16} strokeWidth={1.5} />
      </Button>
    </TooltipTrigger>
    <TooltipContent side="top">Edit booking</TooltipContent>
  </Tooltip>
</TooltipProvider>
```

---

### 4.14 Dropdown Menu (User menu + notification "...")

```tsx
// User menu in topbar
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button variant="ghost-white" className="flex items-center gap-2">
      <AvatarInitials name={userName} size="sm" />
      <span className="text-sm font-medium">{userName} | {branch}</span>
      <ChevronDown size={16} />
    </Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent align="end" className="w-48">
    <DropdownMenuItem>My profile</DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem className="text-destructive focus:text-destructive">
      Log out
    </DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>

// Notification item "..." context menu
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button variant="ghost" size="icon" className="h-7 w-7">
      <MoreHorizontal size={16} strokeWidth={1.5} />
    </Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent align="end">
    <DropdownMenuItem>Mark as read</DropdownMenuItem>
    <DropdownMenuItem>Dismiss</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

---

## 5. CUSTOM COMPONENTS

### 5.1 StatusChip

```tsx
// src/components/ui/status-chip.tsx
import { type HTMLAttributes } from 'react';
import { cn } from '@/lib/utils';

export type BookingStatus =
  | 'for-review'
  | 'confirmed'
  | 'in-transit'
  | 'arrived'
  | 'in-progress'
  | 'completed'
  | 'cancelled'
  | 'waived'
  | 'for-payment-verification';

const STATUS_CONFIG: Record<BookingStatus, { label: string; color: string }> = {
  'for-review':               { label: 'For review',               color: '#EF6C00' },
  'confirmed':                { label: 'Confirmed',                color: '#2E7D32' },
  'in-transit':               { label: 'In transit',               color: '#3886C8' },
  'arrived':                  { label: 'Arrived',                  color: '#3886C8' },
  'in-progress':              { label: 'In progress',              color: '#0288D1' },
  'completed':                { label: 'Completed',                color: '#2E7D32' },
  'cancelled':                { label: 'Cancelled',                color: '#D32F2F' },
  'waived':                   { label: 'Waived',                   color: '#757575' },
  'for-payment-verification': { label: 'For payment verification', color: '#FBC02D' },
};

interface StatusChipProps extends HTMLAttributes<HTMLSpanElement> {
  status: BookingStatus;
}

export function StatusChip({ status, className, ...props }: StatusChipProps) {
  const { label, color } = STATUS_CONFIG[status];
  return (
    <span
      className={cn('inline-flex items-center rounded-full px-3 py-1 text-xs font-medium whitespace-nowrap', className)}
      style={{ color, backgroundColor: color + '20' }}
      {...props}
    >
      {label}
    </span>
  );
}
```

---

### 5.2 AvatarInitials

```tsx
// src/components/ui/avatar-initials.tsx
import { cn } from '@/lib/utils';

const PALETTE = [
  { bg: '#3886C8', text: '#FFFFFF' }, // 0 — primary blue
  { bg: '#2E7D32', text: '#FFFFFF' }, // 1 — success green
  { bg: '#EF6C00', text: '#FFFFFF' }, // 2 — warning orange
  { bg: '#7B1FA2', text: '#FFFFFF' }, // 3 — purple
  { bg: '#D32F2F', text: '#FFFFFF' }, // 4 — error red
  { bg: '#FBC02D', text: '#121212' }, // 5 — secondary dark amber
];

// Deterministic slot: first char of name mod palette length
function getSlot(name: string): number {
  return name.charCodeAt(0) % PALETTE.length;
}

interface AvatarInitialsProps {
  name: string;
  size?: 'sm' | 'md' | 'lg';
  className?: string;
}

const sizeClasses = {
  sm: 'h-7 w-7 text-xs',
  md: 'h-9 w-9 text-sm',
  lg: 'h-11 w-11 text-base',
};

export function AvatarInitials({ name, size = 'md', className }: AvatarInitialsProps) {
  const initials = name.slice(0, 2).toUpperCase();
  const { bg, text } = PALETTE[getSlot(name)];
  return (
    <span
      className={cn(
        'inline-flex items-center justify-center rounded-full font-medium select-none shrink-0',
        sizeClasses[size],
        className,
      )}
      style={{ backgroundColor: bg, color: text }}
      aria-label={name}
    >
      {initials}
    </span>
  );
}
```

---

### 5.3 StepProgressBar

```tsx
// src/components/ui/step-progress-bar.tsx
import { Check } from 'lucide-react';
import { cn } from '@/lib/utils';

export interface Step {
  id: string;
  label: string;
}

interface StepProgressBarProps {
  steps: Step[];
  currentStep: number; // 0-indexed
}

export function StepProgressBar({ steps, currentStep }: StepProgressBarProps) {
  return (
    <nav aria-label="Progress" className="w-full px-4">
      <ol className="flex items-start">
        {steps.map((step, index) => {
          const isCompleted = index < currentStep;
          const isActive    = index === currentStep;
          const isLast      = index === steps.length - 1;

          return (
            <li key={step.id} className={cn('flex flex-col items-center', !isLast && 'flex-1')}>
              <div className="flex w-full items-center">
                {/* Circle */}
                <span
                  className={cn(
                    'flex h-8 w-8 shrink-0 items-center justify-center rounded-full border-2 text-sm font-medium transition-colors',
                    isCompleted && 'border-primary bg-primary text-primary-foreground',
                    isActive    && 'border-primary bg-primary text-primary-foreground',
                    !isCompleted && !isActive && 'border-neutral-300 bg-background text-muted-foreground',
                  )}
                >
                  {isCompleted ? <Check size={14} strokeWidth={2.5} /> : index + 1}
                </span>

                {/* Connector line */}
                {!isLast && (
                  <div
                    className={cn(
                      'h-0.5 flex-1 mx-2',
                      isCompleted ? 'bg-primary' : 'bg-neutral-300',
                    )}
                  />
                )}
              </div>

              {/* Label */}
              <span
                className={cn(
                  'mt-2 text-xs font-medium text-center',
                  (isCompleted || isActive) ? 'text-primary' : 'text-muted-foreground',
                )}
              >
                {step.label}
              </span>
            </li>
          );
        })}
      </ol>
    </nav>
  );
}
```

**Usage:**
```tsx
const BOOKING_STEPS: Step[] = [
  { id: 'start',         label: 'Start' },
  { id: 'client-info',   label: 'Client information' },
  { id: 'service-details', label: 'Service details' },
  { id: 'assign-hm',     label: 'Assign Housemaid' },
  { id: 'summary',       label: 'Summary' },
];

<StepProgressBar steps={BOOKING_STEPS} currentStep={2} />
```

---

### 5.4 KPIStatCard

```tsx
// src/components/dashboard/kpi-stat-card.tsx
import { Card, CardContent } from '@/components/ui/card';
import { cn } from '@/lib/utils';

interface KPIStatCardProps {
  label: string;
  value: string | number;
  className?: string;
}

export function KPIStatCard({ label, value, className }: KPIStatCardProps) {
  return (
    <Card className={cn('relative overflow-hidden rounded-lg shadow-sm', className)}>
      {/* Yellow accent bar — top edge */}
      <div className="absolute inset-x-0 top-0 h-1 bg-secondary" />
      <CardContent className="px-4 pb-4 pt-5">
        <p className="text-sm text-muted-foreground">{label}</p>
        <p className="mt-1 text-2xl font-bold text-foreground">{value}</p>
      </CardContent>
    </Card>
  );
}
```

**Dashboard grid usage:**
```tsx
<div className="grid grid-cols-4 gap-4">
  <KPIStatCard label="Pay BookMaid"  value="Php 12,400" />
  <KPIStatCard label="Pay Cancelled" value="Php 3,200"  />
  <KPIStatCard label="Pay Refund"    value="Php 800"    />
  <KPIStatCard label="Pay Latecomp"  value="Php 1,600"  />
</div>
```

---

### 5.5 EmptyState

```tsx
// src/components/ui/empty-state.tsx
import { Inbox, SearchX, type LucideIcon } from 'lucide-react';
import { cn } from '@/lib/utils';

type EmptyStateVariant = 'empty' | 'no-results';

const ICONS: Record<EmptyStateVariant, LucideIcon> = {
  'empty':      Inbox,
  'no-results': SearchX,
};

interface EmptyStateProps {
  variant?: EmptyStateVariant;
  heading?: string;
  subtext?: string;
  className?: string;
}

export function EmptyState({
  variant = 'empty',
  heading = 'No items found',
  subtext,
  className,
}: EmptyStateProps) {
  const Icon = ICONS[variant];
  return (
    <div className={cn('flex flex-col items-center justify-center py-16 text-center', className)}>
      <Icon size={48} strokeWidth={1.5} className="text-neutral-300 mb-4" />
      <p className="text-base font-medium text-muted-foreground">{heading}</p>
      {subtext && <p className="mt-1 text-sm text-muted-foreground/70">{subtext}</p>}
    </div>
  );
}
```

---

### 5.6 StarRating

```tsx
// src/components/ui/star-rating.tsx
import { Star } from 'lucide-react';
import { cn } from '@/lib/utils';

interface StarRatingProps {
  value: number;    // 0–5
  max?: number;
  size?: number;
  className?: string;
}

export function StarRating({ value, max = 5, size = 16, className }: StarRatingProps) {
  return (
    <span
      className={cn('inline-flex items-center gap-0.5', className)}
      aria-label={`${value} out of ${max} stars`}
    >
      {Array.from({ length: max }, (_, i) => (
        <Star
          key={i}
          size={size}
          strokeWidth={1.5}
          fill={i < value ? '#FFEB3B' : 'none'}
          color={i < value ? '#FFEB3B' : '#BDBDBD'}
        />
      ))}
    </span>
  );
}
```

---

### 5.7 DetailSection

```tsx
// src/components/ui/detail-section.tsx
// Used for: Service details, Transportation summary, Payment summary, Booking summary
import { cn } from '@/lib/utils';

interface DetailSectionProps {
  title: string;
  children: React.ReactNode;
  className?: string;
  action?: React.ReactNode; // optional inline button
}

export function DetailSection({ title, children, className, action }: DetailSectionProps) {
  return (
    <section className={cn('space-y-3', className)}>
      <div className="flex items-center justify-between">
        <h3 className="text-sm font-semibold" style={{ color: '#3886C8' }}>{title}</h3>
        {action}
      </div>
      {children}
    </section>
  );
}

// Row pattern within a section:
// <div className="flex justify-between text-sm">
//   <span className="text-muted-foreground">Service fee</span>
//   <span>Php 1,090.00</span>
// </div>
// <div className="flex justify-between text-sm font-bold border-t pt-2">
//   <span>Total amount due</span>
//   <span>Php 1,090.00</span>
// </div>
```

---

## 6. DASHBOARD LAYOUT SHELL

### 6.1 Route Structure (App Router)

```
src/app/
├── (auth)/
│   ├── layout.tsx            ← AuthPageTemplate (two-column)
│   ├── login/page.tsx
│   ├── otp/page.tsx
│   └── create-password/page.tsx
│
└── (dashboard)/
    ├── layout.tsx            ← DashboardLayout
    ├── dashboard/
    │   └── page.tsx          ← Analytics + KPI cards + charts
    ├── book-housemaid/
    │   └── page.tsx          ← Multi-step form (StepProgressBar)
    ├── bookings/
    │   ├── page.tsx          ← DataTable + FilterBar
    │   └── [id]/
    │       ├── page.tsx      ← Booking detail (Tabs)
    │       └── waive/page.tsx← Waive booking form
    ├── housemaids/
    │   └── page.tsx
    └── booking-history/
        └── page.tsx
```

---

### 6.2 AppTopbar

> **Layout note:** The topbar is white with a bottom border. The logo lives in the sidebar, not
> the topbar. The topbar contains only the page title, search, notifications, and user profile.

```tsx
// src/components/layout/app-topbar.tsx
'use client';

import { Button } from '@/components/ui/button';
import { AvatarInitials } from '@/components/ui/avatar-initials';
import { UserDropdownMenu } from './user-dropdown-menu';
import { NotificationDropdown } from './notification-dropdown';
import { Search } from 'lucide-react';

interface AppTopbarProps {
  pageTitle: string;
  userName: string;
  role?: string;
  notificationCount?: number;
}

export function AppTopbar({ pageTitle, userName, role = 'Super Admin', notificationCount = 0 }: AppTopbarProps) {
  return (
    <header
      className="flex h-[var(--gk-topbar-height)] shrink-0 items-center justify-between px-8 border-b border-border bg-white z-[var(--z-sticky)]"
    >
      {/* Page title */}
      <h2 className="text-lg font-semibold text-slate-800">{pageTitle}</h2>

      {/* Right controls */}
      <div className="flex items-center gap-4">
        {/* Search */}
        <div className="relative">
          <Search size={16} className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" />
          <input
            type="text"
            placeholder="Search..."
            className="h-9 w-64 rounded-md border border-slate-200 bg-slate-50 pl-9 pr-4 text-sm outline-none
                       focus:border-primary focus:ring-1 focus:ring-ring/30 placeholder:text-slate-400"
          />
        </div>

        {/* Notifications */}
        <NotificationDropdown count={notificationCount} />

        <div className="h-8 w-px bg-slate-200" />

        {/* User */}
        <div className="flex items-center gap-3">
          <div className="text-right hidden sm:block">
            <p className="text-sm font-medium text-slate-900">{userName}</p>
            <p className="text-xs text-slate-500">{role}</p>
          </div>
          <UserDropdownMenu userName={userName} />
        </div>
      </div>
    </header>
  );
}
```

---

### 6.3 AppSidebar

> **Layout note:** The logo is in the sidebar header, not the topbar. Active nav item uses
> yellow (`#FFEB3B` / `bg-secondary`) background with dark text (`#0F172A` / `text-secondary-foreground`).
> Nav items are grouped under section headings (Overview / Management / Finance).

```tsx
// src/components/layout/app-sidebar.tsx
'use client';

import Link from 'next/link';
import { usePathname } from 'next/navigation';
import {
  LayoutDashboard,
  BarChart2,
  CalendarPlus,
  Users,
  User,
  CreditCard,
  Receipt,
  LogOut,
} from 'lucide-react';
import { cn } from '@/lib/utils';

const NAV_GROUPS = [
  {
    label: 'Overview',
    items: [
      { href: '/dashboard',  label: 'Dashboard',  Icon: LayoutDashboard },
      { href: '/analytics',  label: 'Analytics',  Icon: BarChart2       },
    ],
  },
  {
    label: 'Management',
    items: [
      { href: '/bookings',    label: 'Bookings',    Icon: CalendarPlus },
      { href: '/housemaids',  label: 'Housemaids',  Icon: Users        },
      { href: '/customers',   label: 'Customers',   Icon: User         },
    ],
  },
  {
    label: 'Finance',
    items: [
      { href: '/transactions', label: 'Transactions', Icon: CreditCard },
      { href: '/invoices',     label: 'Invoices',     Icon: Receipt    },
    ],
  },
] as const;

export function AppSidebar() {
  const pathname = usePathname();

  return (
    <aside
      className="flex shrink-0 flex-col border-r border-slate-200 bg-white"
      style={{ width: 'var(--gk-sidebar-width)' }}
    >
      {/* Logo — matches topbar height so they align */}
      <div className="flex h-[var(--gk-topbar-height)] items-center border-b border-slate-100 px-6">
        <div className="flex items-center gap-2">
          <div className="rounded-md bg-primary px-2 py-1">
            <span className="text-xl font-extrabold tracking-tight leading-none">
              <span style={{ color: '#FFEB3B' }}>GET</span>
              <span className="text-white">KLEAN</span>
            </span>
          </div>
        </div>
      </div>

      {/* Nav */}
      <nav className="flex-1 overflow-y-auto px-4 py-6">
        {NAV_GROUPS.map((group) => (
          <div key={group.label} className="mb-6">
            <p className="mb-2 px-2 text-[10px] font-bold uppercase tracking-wider text-slate-500">
              {group.label}
            </p>
            <ul className="space-y-0.5">
              {group.items.map(({ href, label, Icon }) => {
                const isActive = pathname === href || pathname.startsWith(href + '/');
                return (
                  <li key={href}>
                    <Link
                      href={href}
                      className={cn(
                        'flex items-center gap-3 rounded-md px-3 py-2 text-sm font-medium transition-colors',
                        isActive
                          ? 'bg-secondary text-secondary-foreground font-semibold'
                          : 'text-slate-800 hover:bg-neutral-100 hover:text-slate-900',
                      )}
                    >
                      <Icon size={20} strokeWidth={1.5} className="shrink-0" />
                      {label}
                    </Link>
                  </li>
                );
              })}
            </ul>
          </div>
        ))}
      </nav>

      {/* Sign out */}
      <div className="border-t border-slate-100 p-4">
        <Link
          href="/logout"
          className="flex items-center gap-3 rounded-md px-3 py-2 text-sm font-medium text-red-600
                     transition-colors hover:bg-red-50 hover:text-red-700"
        >
          <LogOut size={20} strokeWidth={1.5} className="shrink-0" />
          Sign Out
        </Link>
      </div>
    </aside>
  );
}
```

---

### 6.4 AppFooter

```tsx
// src/components/layout/app-footer.tsx
export function AppFooter() {
  return (
    <footer
      className="flex shrink-0 flex-col items-center justify-center gap-1 py-3 text-center text-xs"
      style={{ backgroundColor: 'var(--gk-footer-bg)', color: 'rgba(255,255,255,0.60)' }}
    >
      <p>© 2024 GetKlean PH. All rights reserved.</p>
      <div className="flex gap-4">
        <a href="/terms"     className="hover:text-white transition-colors">Terms and Conditions</a>
        <a href="/privacy"   className="hover:text-white transition-colors">Data Privacy Policy</a>
        <a href="/insurance" className="hover:text-white transition-colors">Housemaid Insurance Policy</a>
      </div>
    </footer>
  );
}
```

---

### 6.5 DashboardLayout (App Router)

```tsx
// src/app/(dashboard)/layout.tsx
import { AppTopbar } from '@/components/layout/app-topbar';
import { AppSidebar } from '@/components/layout/app-sidebar';
import { AppFooter } from '@/components/layout/app-footer';

export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen overflow-hidden bg-[#F8FAFC]">
      {/* Sidebar (fixed left, full height, contains logo) */}
      <AppSidebar />

      {/* Right column: topbar + scrollable main + footer */}
      <div className="flex flex-1 flex-col overflow-hidden">
        <AppTopbar pageTitle="Dashboard Overview" userName="Juan dela Cruz" role="Super Admin" notificationCount={3} />

        <main
          className="flex-1 overflow-y-auto"
          style={{ padding: 'var(--gk-content-padding)' }}
        >
          {children}
        </main>

        <AppFooter />
      </div>
    </div>
  );
}
```

---

### 6.6 Page Layout Patterns

**Standard page (list / analytics):**
```tsx
export default function BookingsPage() {
  return (
    <div className="space-y-6">
      <h1 className="text-xl font-medium">Bookings</h1>
      <FilterBar />
      <BookingsTable />
    </div>
  );
}
```

**Detail page (back navigation + action button):**
```tsx
export default function BookingDetailPage({ params }: { params: { id: string } }) {
  return (
    <div className="space-y-6">
      {/* Header row */}
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-1">
          <Link href="/bookings">
            <Button variant="ghost" size="icon" className="-ml-2">
              <ArrowLeft size={20} strokeWidth={1.5} />
            </Button>
          </Link>
          <h1 className="text-xl font-medium">{params.id}</h1>
        </div>
        <Button variant="destructive-outlined" size="md">Cancel booking</Button>
      </div>

      {/* Tabs */}
      <TabbedPanel bookingId={params.id} />
    </div>
  );
}
```

---

## 7. COMPONENT STATE REFERENCE

### Universal state → Tailwind class map

| State | Tailwind |
|---|---|
| Hover — primary surface | `hover:bg-primary/90` or `hover:bg-primary-dark` |
| Hover — ghost / light | `hover:bg-black/4` |
| Hover — input/select border | `hover:border-primary` |
| Focus ring | `focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring/30 focus-visible:ring-offset-2` |
| Active nav item | `bg-secondary text-secondary-foreground font-semibold` (`#FFEB3B` / `#0F172A`) |
| Disabled | `disabled:pointer-events-none disabled:opacity-50` |
| Disabled button (MUI pattern) | `disabled:bg-black/12 disabled:text-black/26` |
| Error border | `aria-[invalid=true]:border-destructive` |
| Error text | `text-destructive text-xs` |
| Loading | `<Loader2 size={20} className="animate-spin" />` |
| Card lift on hover | `hover:-translate-y-0.5 hover:shadow-md transition-all duration-300` |
| Underline tab active | `data-[state=active]:border-b-2 data-[state=active]:border-primary data-[state=active]:text-primary` |
| Selected checkbox/radio | `data-[state=checked]:bg-primary data-[state=checked]:border-primary` |

### Transition defaults (already in globals.css @layer base)

All interactive elements get:
```css
transition-property: color, background-color, border-color, box-shadow, transform;
transition-duration: 150ms;
transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
```

Card hover: `transition-all duration-300 ease-in-out`

---

## 8. IMPLEMENTATION CHECKLIST

### Phase 1 — Foundation
- [ ] Copy §2 CSS into `src/app/globals.css`
- [ ] Copy §3 config into `tailwind.config.ts`
- [ ] Install Montserrat: `next/font/google` → `const montserrat = Montserrat({ subsets: ['latin'] })`
- [ ] Run `npx shadcn@latest init` — choose: TypeScript, Tailwind, App Router
- [ ] Install `lucide-react`: `npm install lucide-react`

### Phase 2 — shadcn components
- [ ] `button` · `input` · `label` · `form`
- [ ] `card` · `alert`
- [ ] `dialog`
- [ ] `table`
- [ ] `tabs` · `accordion`
- [ ] `select` · `checkbox` · `radio-group` · `textarea`
- [ ] `tooltip` · `popover` · `dropdown-menu`
- [ ] `badge` · `pagination` · `separator` · `avatar` · `skeleton` · `sheet`

### Phase 3 — Custom components
- [ ] `StatusChip` — §5.1
- [ ] `AvatarInitials` — §5.2
- [ ] `StepProgressBar` — §5.3
- [ ] `KPIStatCard` — §5.4
- [ ] `EmptyState` — §5.5
- [ ] `StarRating` — §5.6
- [ ] `DetailSection` — §5.7

### Phase 4 — Layout shell
- [ ] `AppTopbar` — §6.2
- [ ] `AppSidebar` — §6.3
- [ ] `AppFooter` — §6.4
- [ ] `(dashboard)/layout.tsx` — §6.5
- [ ] `(auth)/layout.tsx` — AuthPageTemplate (two-column grid)

### Phase 5 — Pages
- [ ] `/dashboard` — KPI grid + chart placeholders
- [ ] `/bookings` — DataTable + FilterBar + StatusChip
- [ ] `/bookings/[id]` — TabbedPanel + DetailSection + Accordion
- [ ] `/bookings/[id]/waive` — WaiveBookingForm
- [ ] `/book-housemaid` — StepProgressBar + multi-step form
- [ ] `/housemaids` — list view
- [ ] `/booking-history` — list view

### Phase 6 — Charts
- [ ] `npm install recharts`
- [ ] `ChartCard` wrapper component (bar: `#3886C8` + `#FFEB3B`, line, donut)

### Phase 7 — Polish
- [ ] Skeleton loading states for table rows and cards
- [ ] Confirm all `focus-visible` rings work (accessibility check)
- [ ] Check `data-theme="dark"` toggling if dark mode is added later
- [ ] Validate all StatusChip colors render correctly

---

*Source: `DESIGN-SYSTEM-SPEC.md` — GKAdmin-feature-86cyyqrmk-create-account-ui*
*Generated: 2026-02-18*
