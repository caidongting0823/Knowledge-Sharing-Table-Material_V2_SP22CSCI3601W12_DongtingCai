# Knowledge Sharing - Table Material

**Spring 2022 / CSCI 3601 / Week 12 / Apr 12**  
***By Dongting Cai (David)***

<br />
### Related Links:
> The Case by Rocinante in Iteration 2 Github Link:   
https://github.com/UMM-CSci-3601-S22/it-2-rocinante/tree/main/client/src/app/pantry  
> Official Case of Table with expandable rows: https://material.angular.io/components/table/examples#table-expandable-rows  
> Official Document of API reference for Angular Material table: https://material.angular.io/components/table/api  

<br />

### The Related Code from the Case by Rocinante in Iteration 2
```HTML
//.html
<table mat-table
        [dataSource]="productCategory.value" multiTemplateDataRows
        [ngClass]="productCategory.key.replace(' ', '-') + '-table'"
        class="mat-elevation-z8">
    <ng-container matColumnDef="{{column}}" *ngFor="let column of displayedColumns">
    <th mat-header-cell *matHeaderCellDef class="capitalize"> {{column}} </th>
    <td mat-cell *matCellDef="let element"> {{element[column]}} </td>
    </ng-container>

    <!-- Expanded Content Column - The detail row is made up of this one column that spans across all columns -->
    <ng-container matColumnDef="expandedDetail">
    <td mat-cell *matCellDef="let element of expandedDetail" [attr.colspan]="displayedColumns.length">
        <div class="pantry-detail"
            [@detailExpand]="element === expandedElement ? 'expanded' : 'collapsed'">
        <div class="pantry-detail-description" style="margin: auto; text-align: center;">
            {{element.notes}}
        </div>
        </div>
    </td>
    </ng-container>

    <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
    <tr mat-row *matRowDef="let element; columns: displayedColumns;"
    class="pantry-element-row"
    [class.pantry-expanded-row]="expandedElement === element"
    (click)="expandedElement = expandedElement === element ? null : element">
    </tr>
    <tr mat-row *matRowDef="let row; columns: ['expandedDetail']" class="pantry-detail-row"></tr>
</table>
```
<br />
### Generate Basic Tables from Data

`[dataSource]`   
Definitions within this data source are defined by variables within the corresponding `.ts` file. In Angular's original collapsed table paradigm, the corresponding code looks like this:
```typescript
// example.component.html
[dataSource]="dataSource"

// example.component.ts
export class TableExpandableRowsExample {
  dataSource = ELEMENT_DATA;  // This defines the source of the data in the table
  columnsToDisplay = ['name', 'weight', 'symbol', 'position'];
  expandedElement: PeriodicElement | null;
}
...
const ELEMENT_DATA: PeriodicElement[] = [
  {
    position: 1,
    name: 'Hydrogen',
    weight: 1.0079,
    symbol: 'H',
    description: `Hydrogen is a chemical element with symbol H and atomic number 1. With a standard
        atomic weight of 1.008, hydrogen is the lightest element on the periodic table.`,
  },
  ...
]
```
In our practical application, because we wrap a layer of collapsible panels sorted by item category outside the Table, our data acquisition source is `productCategory.value`.

`multiTemplateDataRows` 
The `multiTemplateDataRows` property is added on the `<table>` element. This property remains `false` by default.  By adding it allows the Data object to add multiple rows in the datatable.a

```HTML
<ng-container matColumnDef="{{column}}" *ngFor="let column of displayedColumns">
```
This is an essential line because this line specifies how to generate the corresponding table based on the related data. `column` will be a variable referring to the column in the HTML file, and `displayedColumns` is determined by the corresponding code in `component.ts`. In our actual use, the corresponding code of `.ts` looks like this:
```typescript
displayedColumns: string[] = ['productName', 'brand', 'purchase_date'];
```
Please note that the content formulated here must be the same as the corresponding data title in `JSON(database)`, otherwise the project will not be able to obtain the related data, so the table cannot be generated directly.

<br />

### Folded Part
In the official project example, this part of the code becomes a bit more complicated because of the addition of graphics for chemical elements. But the official case is very interesting for implementing this graph - if you're interested, go look it up.
The content we implement here is relatively simple. Add the `notes` field in `PantryItem` here, and click the corresponding data in the table to display the notes content of the corresponding product in the pantry.

The `[@detailExpand]` is the animation trigger with two states `expanded` and `collapsed` determined by the isExpanded boolean property.  

<br />

### Setting of the Overall Format of the Table and Related Settings
In the last part, here are mainly the settings for the inside of the table, especially the folded part. The most notable is `class="pantry-element-row"`, controlled by the corresponding code in the `.scss` file, determines whether there is data, and displays the folded content according to the actual situation. It is worth mentioning that if there is no related data in a folded row, the row will not respond when clicked. The content of the corresponding control in `.scss` in our case is as follows:
```css
tr.pantry-element-row:not(.pantry-expanded-row):hover {
  background: whitesmoke;
}

tr.pantry-element-row:not(.pantry-expanded-row):active {
  background: #efefef;
}
```

This is basically what we use for the table. Compared with the official folding table case, the code in our project deletes the content we will not use, making the table more concise and in line with our actual needs. Hopefully it helped you. : )
