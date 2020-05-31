---
# required metadata

title: Lookup form pattern
description: This topic provides information about the Lookup form pattern. 
author: jasongre
manager: AnnBe
ms.date: 11/09/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-platform
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: Developer
# ms.devlang: 
ms.reviewer: robinr
ms.search.scope: Operations
# ms.tgt_pltfrm: 
ms.custom: 12911
ms.assetid: 0bc7bde2-6150-4a80-8738-9a5201b51df2
ms.search.region: Global
# ms.search.industry: 
ms.author: jasongre
ms.search.validFrom: 2016-02-28
ms.dyn365.ops.version: AX 7.0.0

---

# Lookup form pattern

[!include [banner](../includes/banner.md)]

This topic provides information about the Lookup form pattern. Custom lookup forms should be used when a standard framework-provided lookup would not provide the correct data, or when advanced visualization of the data is required.

Usage
-----

Custom lookup forms should be used when a standard framework-provided lookup (which is typically generated by using the AutoLookup field group that is defined on the table definition), would not provide the correct data, or when advanced visualization of the data is required. Three patterns are described in this document:

-   **Lookup basic** – This is the basic Lookup pattern that has just one list or tree, and also optional custom filters and actions.
-   **Lookup w/tabs** – This Lookup pattern is used when more than one view of the lookup can be made available to the user. Tab captions aren't shown. Instead, the tab is selected through a combo box.
-   **Lookup w/preview** – This more advanced Lookup pattern enables a preview of the current record in the lookup grid.

## Wireframe
### Lookup basic

[![LookupForm(1)](./media/lookupform1.png)](./media/lookupform1.png)

### Lookup w/tabs

[![LookupForm(2)](./media/lookupform2.png)](./media/lookupform2.png)

### Lookup w/preview

[![LookupForm(3)](./media/lookupform3.png)](./media/lookupform3.png)

## Pattern changes
Here are the changes to this pattern since Microsoft Dynamics AX 2012:

-   Tabs should be hidden and controlled by a combo box.
-   Optionally add a splitter/preview.

## Model
### Lookup basic – High-level structure

- Design

    - *CustomFilter (Group) \[Optional\]*
    - Grid | Tree | ListView
    - *LookupActions (Group) \[Optional\]*

### Lookup w/tabs – High-level structure

- Design

    - *CustomFilter (Group) \[Optional\]*
    - LookupTab (Tab)

        - LookupTabPage (TabPage, repeats 1..N)

            - Grid | Tree | ListView

    - *LookupActions (Group) \[Optional\]*

### Lookup w/preview – High-level structure

- Design

    - *CustomFilter (Group) \[Optional\]*
    - LookupContent (Group)

        - Grid | Tree | ListView

    - VerticalSplitter (Group)

        - Preview (Group)

    - LookupActions (ActionPane)

### Core components

-   Apply the Lookup pattern on Form.Design.
-   Address BP Warnings:
    -   EDT.FormHelp must reference a form where Style=Lookup.

### Commonly used subpatterns

-   [Custom Filter Group](custom-filter-group-subpattern.md)

## UX guidelines
The verification checklist shows the steps for manually verifying that the form complies with UX guidelines. This checklist doesn't include any guidelines that will be enforced automatically through the development environment. Open the form in a browser, and walk through these steps. **Standard form guidelines**

-   Standard form guidelines have been consolidated into the [General Form Guidelines](general-form-guidelines.md) document.

**Lookup guidelines**

-   **Grid** guidelines have been consolidated into the [General Form Guidelines ](general-form-guidelines.md) document, in the Grid guidelines section.
-   If you must show different “views” (tabs) within the lookup, use a combo box to let the user to switch between tabs.
-   You can optionally use a tree view in the lookup. Also consider providing a standard grid because of the complexity that is involved in showing additional fields of data in a tree.
-   Don't have more than five columns in the grid. The lookup resizes to show all columns, so five columns is very wide.
-   The **optional preview area**:
    -   The area should help the user choose between two or more records that are similar. For example, if you have two employees who are named John Smith, the preview should provide enough information to help the user differentiate these two people.
    -   Don't show editable fields in the preview.
-   **Custom filter** guidelines have been consolidated into the [Custom Filter Group](custom-filter-group-subpattern.md) subpattern document.

## Examples
### Lookup basic

Form: **SysLanguageLookup** (Click **Settings** &gt; **User settings** on the navigation bar.) 

[![LookupForm(4)](./media/lookupform4.png)](./media/lookupform4.png)

### Lookup w/tabs

Form: **CaseCategoryLookup** (Click **Common** &gt; **Common** &gt; **Cases** &gt; **All cases**, and then select a case to go to the details.) 

![LookupForm(5)](./media/lookupform5.png)

### Lookup w/preview

Form: **HcmWorkerLookup** (Click **Human resources** &gt; **Common** &gt; **Organization** &gt; **Positions** &gt; **Positions**, and then click a record to go to the details. Expand the **Worker assignment** FastTab, click **New**, and then click the drop-down arrow in the **Worker** field.) 

[![LookupForm(6)](./media/lookupform6.png)](./media/lookupform6.png)

## Appendix
### Frequently asked questions

This section will have answers to frequently asked questions that are related to this guideline/pattern.

-   **How do I switch between tabs in the Lookup w/tabs pattern?**
    -   The Lookup w/tabs pattern intentionally sets **ShowTabs** to **No** on the Tab control. These forms are meant to model an unbound combo box in the custom filter group. This combo box is used instead of tab headers to switch tabs.
    -   To do this, follow these steps:
        1.  Call the **SysLookup::tab2ComboBox** method post **super** in form **run()** to populate the combo box with captions from visible tabs in the lookup.

                // Generate view combobox based on tabs
                tab2ComboBoxItemMap = SysLookup::tab2ComboBox(Tab, switchView);

        2.  Override **modified()** on the combo box to update the visible tab, based on the selected value in the combo box.

                Tab.tabChanged(Tab.tabValue(), tab2ComboBoxItemMap.lookup(this.selection()));

### Open issues

-   **Can we incorporate the most recently used values into lookups?**
    -   App modeling can make this work right now (for example, the Currency lookup). We are considering general framework support for this feature in the future.

### AX 2012 content

**SysLanguageLookup (Lookup basic)** 

![LookupForm(7)](./media/lookupform7.png) 

**CaseCategoryLookup (Lookup w/tabs)** 

[![LookupForm(8)](./media/lookupform8.png)](./media/lookupform8.png) 

**HcmWorkerLookup (Lookup w/preview)** 

![LookupForm(9)](./media/lookupform9.png)