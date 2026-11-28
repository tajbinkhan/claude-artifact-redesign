# UX and UI rules

The builder follows these rules and the reviewer checks against them. Each rule has an ID so a
review can cite it, for example "V4 broken: two primary buttons in the ticket header".

Pattern rules (P) apply when the screen has that kind of content or task. Every other rule
applies to every screen. Break a rule only when the product needs it, and say why in the
delivery.

## P. Patterns, chosen by the task

- **P1. Navigation.** Use a left sidebar for apps with 5 or more sections. Use tabs for 2 to 5
  sibling views of one object. Add breadcrumbs once pages nest more than 2 levels deep. Always
  highlight where the user is.
- **P2. List and detail.** For inboxes, tickets, contacts and other record lists, show the list
  on the left and the selected item on the right. On mobile they stack, and a back control
  returns to the list with the scroll position kept.
- **P3. Overview, then filter, then details.** Show the big picture first, let users narrow it,
  and reveal details when asked.
- **P4. Attention first.** A dashboard opens with what needs action, for example "3 tickets
  breach SLA in 1 hour". Totals come second. No vanity metrics.
- **P5. Progressive disclosure.** Show the common options and put advanced ones behind "More
  options". Defaults fit most users.
- **P6. Tables.** Use a table when users compare records. Columns sort, filters sit above the
  table, the header stays visible on scroll, and bulk actions appear only after rows are
  selected. Clicking a row opens its detail.
- **P7. Forms.** One column, labels above fields, required fields marked. Validate a field when
  the user leaves it and show the error next to it with the fix. Buttons name the action, like
  "Save changes", never "Submit". Keep the user's input after an error.
- **P8. Destructive actions.** An action that can be undone happens at once and shows a toast
  with Undo. An action that cannot be undone asks first and names the object, for example
  "Delete 12 contacts? This can't be undone."
- **P9. Modal or page.** A modal is for short, focused tasks. A long task gets its own page.
  Never open a modal from a modal.
- **P10. Empty states.** Say what will appear here and give the action that fills it. A
  filtered view with no results offers to clear the filters.
- **P11. Search and shortcuts.** Apps with many records or sections get global search and a
  Ctrl/Cmd+K command palette. Frequent actions get keyboard shortcuts, listed in their tooltips.

## U. UX rules

- **U1. Nielsen's 10 heuristics.** Show system status. Use the user's words. Let users undo and
  leave. Stay consistent. Prevent errors. Let users recognize instead of recall. Give experts
  shortcuts. Keep it minimal. Write errors that help recovery. Put help where it is needed.
- **U2. Hick's law.** Each added choice slows the decision. Cut options before styling them.
- **U3. Fitts's law.** Frequent actions get large targets close to where attention already is.
- **U4. Jakob's law.** Users expect the app to work like the apps they already use. Follow
  common conventions unless breaking one clearly helps the user.
- **U5. Response time.** Give visible feedback within 100ms. Show a skeleton that matches the
  layout when loading takes longer than 300ms. Disable a button while its request runs so it
  cannot be sent twice, and show progress on it.
- **U6. Status.** Every async action ends in a visible result, success or error. Nothing fails
  silently.

## V. Visual rules

- **V1. Spacing.** Use a 4/8px scale. Related items sit closer together than unrelated ones.
- **V2. Type.** 5 or 6 sizes at most. Body text 14 to 16px, line height 1.4 to 1.6, text lines
  45 to 75 characters long. Tabular numerals in tables and metrics.
- **V3. Color.** A neutral base with one accent color for actions. Success, warning, danger and
  info colors carry meaning only, never decoration. Status never relies on color alone, so pair
  it with text or an icon.
- **V4. One primary action.** Each view or section has one primary button. Everything else is
  secondary or tertiary.
- **V5. Alignment.** Align to a grid. Left-align text, right-align numbers.
- **V6. Icons.** Label every icon unless its meaning is universal, such as search or close.
  Icon-only buttons have an accessible name and a tooltip.
- **V7. Motion.** 150 to 250ms, used only to explain a change. Respect
  `prefers-reduced-motion`.
- **V8. Tokens.** Colors, spacing, radii and type are tokens, so dark mode is a token swap and
  not a second design.

## A. Accessibility (WCAG 2.2 AA)

- **A1. Contrast.** 4.5:1 for text. 3:1 for large text, icons and UI component boundaries.
- **A2. Keyboard.** Everything works from the keyboard in a logical order. Focus rings are always
  visible. A skip link comes first on the page.
- **A3. Modals.** A modal traps focus while open, closes on Escape, and returns focus to the
  control that opened it.
- **A4. Markup.** Semantic HTML (buttons are `<button>`, links are `<a>`, headings in order). Every
  input has a real label, never only a placeholder. Toasts and live updates use `aria-live`.
- **A5. Targets.** At least 24 by 24px, and 44px on touch screens.

## R. Responsive rules

- **R1. Widths.** Mobile first. Check 375px, 768px and 1280px. The page never scrolls sideways.
- **R2. Tables on phones.** Rows become cards, or the table scrolls inside its own container.
- **R3. No hover-only actions.** Touch screens have no hover, so every hover action also has a
  visible or tap control.

## C. Copy rules

- **C1. Case and verbs.** Sentence case for headings and labels. Buttons start with a verb.
- **C2. Product words.** Use the product's own terms, such as "ticket" or "workspace", the same
  way everywhere.
- **C3. Errors.** Say what happened and how to fix it. "Something went wrong" alone is not an
  error message.

## X. Anti-patterns to reject

- **X1.** Carousels or tabs that hide information the user needs at once.
- **X2.** Unlabeled icons whose meaning users must guess.
- **X3.** Disabled buttons with no reason shown.
- **X4.** Placeholders used as the only label.
- **X5.** Confirm-shaming copy, like "No thanks, I don't want to save money".
- **X6.** Decoration with no job, such as gradient text, glows, decorative charts and emoji labels.
- **X7.** Controls that look interactive but do nothing.
