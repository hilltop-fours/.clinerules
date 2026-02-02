# Storybook Conventions

## Story Structure
- One story file per component
- Use clear, descriptive story names
- Group related stories using subgroups

## Story Format
- Use CSF3 (Component Story Format 3) format
- Export stories as named exports
- Use the `meta` object for component-level configuration

## Documentation
- Add descriptions to stories to explain component usage
- Document component props and their purposes
- Include examples of different component states

## Best Practices
- Show all meaningful variations of a component
- Include interactive controls for props
- Demonstrate edge cases and error states
- Keep stories focused and simple

## Example Story Structure
```typescript
import type { Meta, StoryObj } from '@storybook/angular';
import { MyComponent } from './my-component';

const meta: Meta<MyComponent> = {
  title: 'Components/MyComponent',
  component: MyComponent,
  tags: ['autodocs'],
};

export default meta;
type Story = StoryObj<MyComponent>;

export const Default: Story = {
  args: {
    // default props
  },
};

export const WithCustomProps: Story = {
  args: {
    // custom props
  },
};
```
