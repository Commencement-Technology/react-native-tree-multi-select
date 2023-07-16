# react-native-tree-multi-select

Super-fast Tree view with multi-selection capabilities, using checkboxes and search filtering.

[![npm version](https://img.shields.io/npm/v/react-native-tree-multi-select)](https://badge.fury.io/js/react-native-tree-multi-select) ![License](https://img.shields.io/github/license/JairajJangle/react-native-tree-multi-select) ![Workflow Status](https://github.com/JairajJangle/react-native-tree-multi-select/actions/workflows/ci.yml/badge.svg) ![Static Badge](https://img.shields.io/badge/platform-android%20%26%20ios-blue) ![GitHub issues](https://img.shields.io/github/issues/JairajJangle/react-native-tree-multi-select)

<div style="display: flex; justify-content: space-around;">
  <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNHFleDNleTZsMXVoMjk1YnlpdXFtanZyZGprMDkwcDdteGhqYTNhcCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/L0w26RrC32gdfWZ8Ux/giphy.gif" alt="demo" style="border: 1px solid gray;" />
  <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExdGxuZHNqaGhrZmdyZzRtY21icHNtbHZoM3N4aHlyMDFxZjJrd25rMyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/KY6Y0gkSPYAFxffL8r/giphy.gif" alt="demo2" style="border: 1px solid gray;" />
</div>


## Installation

Using yarn 

```sh
yarn add react-native-tree-multi-select
```

using npm:

```sh
npm install react-native-tree-multi-select
```

Dependencies that need to be installed for this library to work:

1. [@shopify/flash-list](https://github.com/Shopify/flash-list)
2. [react-native-paper](https://github.com/callstack/react-native-paper)
3. [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons)

Make sure to follow the native-related installation instructions for these dependencies.

## Usage

```tsx
import {
  TreeView,
  type TreeNode,
  type TreeViewRef
} from 'react-native-tree-multi-select';

// Refer to the Properties table below or the example app for the TreeNode type
const myData: TreeNode[] = [...];

export function TreeViewUsageExample(){
  const treeViewRef = React.useRef<TreeViewRef | null>(null);
  
  // It's recommended to use debounce for the search function (refer to the example app)
  function triggerSearch(text: string){
    // Pass search text to the tree along with the keys on which search is to be done(optional)
    treeViewRef.current?.setSearchText(text, ["name"]);
  }
  
  // Callback functions for check and expand state changes:
  const handleSelectionChange = (checkedIds: string[]) => {
    // NOTE: Do something with updated checkedIds here
  };
  const handleExpanded = (expandedIds: string[]) => {
    // NOTE: Do something with updated expandedIds here
  };

  // Expand collapse calls using ref
  const expandAllPress = () => treeViewRef.current?.expandAll?.();
  const collapseAllPress = () => treeViewRef.current?.collapseAll?.();

  // Multi-selection function calls using ref
  const onSelectAllPress = () => treeViewRef.current?.selectAll?.();
  const onUnselectAllPress = () => treeViewRef.current?.unselectAll?.();
  const onSelectAllFilteredPress = () => treeViewRef.current?.selectAllFiltered?.();
  const onUnselectAllFilteredPress = () => treeViewRef.current?.unselectAllFiltered?.();
  
  return(
    // ... Remember to keep a fixed height for the parent. Read Flash List docs to know why
    <TreeView
      ref={treeViewRef}
      data={myData}
      onCheck={handleSelectionChange}
      onExpand={handleExpanded}
    />
  );
}
```

### Properties

| Property                           | Type                                                         | Required | Description                                                  |
| ---------------------------------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| `data`                             | `TreeNode[]`                                                 | Yes      | An array of `TreeNode` objects                               |
| `onCheck`                          | `(checkedIds: string[]) => void`                             | No       | Callback when a checkbox is checked                          |
| `onExpand`                         | `(expandedIds: string[]) => void`                            | No       | Callback when a node is expanded                             |
| `preselectedIds`                   | `string[]`                                                   | No       | An array of `id`s that should be preselected                 |
| `indentationMultiplier`            | `number`                                                     | No       | Indentation (`marginStart`) per level (defaults to 15)       |
| `treeFlashListProps`               | `TreeFlatListProps`                                          | No       | Props for the flash list                                     |
| `checkBoxViewStyleProps`           | `CheckBoxViewStyleProps`                                     | No       | Props for the checkbox view                                  |
| `CheckboxComponent`                | `ComponentType<CheckBoxViewProps>`                           | No       | A custom checkbox component. Defaults to React Native Paper's Checkbox |
| `ExpandCollapseIconComponent`      | `ComponentType<ExpandIconProps>`                             | No       | A custom expand/collapse icon component                      |
| `ExpandCollapseTouchableComponent` | `ComponentType<TouchableOpacityProps>`<br />(React Native's `TouchableOpacityProps`) | No       | A custom expand/collapse touchable component                 |

#### TreeNode

| Property        | Type         | Required | Description                                                  |
| --------------- | ------------ | -------- | ------------------------------------------------------------ |
| `id`            | `string`     | Yes      | Unique identifier for the node                               |
| `name`          | `string`     | Yes      | The display name of the node                                 |
| `children`      | `TreeNode[]` | No       | An array of child `TreeNode` objects                         |
| `[key: string]` | `any`        | No       | Any additional properties for the node <br />(May be useful to perform search on) |

#### TreeViewRef

| Property              | Type                                                  | Description                                                  |
| --------------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| `selectAll`           | `() => void`                                          | Selects **all** nodes                                        |
| `unselectAll`         | `() => void`                                          | Unselects **all** nodes                                      |
| `selectAllFiltered`   | `() => void`                                          | Selects all **filtered** nodes                               |
| `unselectAllFiltered` | `() => void`                                          | Unselects all **filtered** nodes                             |
| `expandAll`           | `() => void`                                          | Expands all nodes                                            |
| `collapseAll`         | `() => void`                                          | Collapses all nodes                                          |
| `setSearchText`       | `(searchText: string, searchKeys?: string[]) => void` | Set the search text and optionally the search keys. Default search key is "name"<br /><br />Recommended to call this inside a debounced function if you find any performance issue otherwise. |

#### CheckBoxViewStyleProps

| Property                   | Type                             | Required | Description                                            |
| -------------------------- | -------------------------------- | -------- | ------------------------------------------------------ |
| `outermostParentViewStyle` | `StyleProp<ViewStyle>`           | No       | Optional style modifier for the outermost parent view. |
| `checkboxParentViewStyle`  | `StyleProp<ViewStyle>`           | No       | Optional style modifier for the checkbox parent view.  |
| `textTouchableStyle`       | `StyleProp<ViewStyle>`           | No       | Optional style modifier for the text touchable style.  |
| `checkboxProps`            | `CheckboxProps`                  | No       | Optional props for the checkbox component.             |
| `textProps`                | `TextProps` <br />(React Native) | No       | Optional props for the text component.                 |

#### CheckboxProps

All properties of `RNPaperCheckboxAndroidProps`(from `react-native-paper`) except for `onPress` and `status`

#### TreeFlatListProps

All properties of `FlashListProps`(from `@shopify/flash-list`) except for `data` and `renderItem`

#### ExpandIconProps

| Property   | Type    | Required | Description                       |
| ---------- | ------- | -------- | --------------------------------- |
| isExpanded | boolean | Yes      | Indicates if the icon is expanded |

---

More customization options are on the way 🙌

---

## Contributing

See the [contributing guide](CONTRIBUTING.md) to learn how to contribute to the repository and the development workflow.

## License

MIT

## Support the project

<p align="center" valign="center">
  <a href="https://liberapay.com/FutureJJ/donate">
    <img src="https://liberapay.com/assets/widgets/donate.svg" alt="LiberPay_Donation_Button" height="50" > 
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <a href=".github/assets/Jairaj_Jangle_Google_Pay_UPI_QR_Code.jpg">
    <img src=".github/assets/upi.png" alt="Paypal_Donation_Button" height="50" >
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://www.paypal.com/paypalme/jairajjangle001/usd">
    <img src=".github/assets/paypal_donate.png" alt="Paypal_Donation_Button" height="50" >
  </a>
</p>


---

Made with [create-react-native-library](https://github.com/callstack/react-native-builder-bob)
