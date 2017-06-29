# DataTable

Data table using bulma (minimally), can sort, filter, and use templates. Not recomended for more than 1000 rows.

![Formatted Table](https://github.com/aislasq/vue-bulma-datatable/blob/master/images/format.png "Formatted Table")

## Installation
```
$ npm install https://github.com/aislasq/vue-bulma-datatable.git
```

## Usage

#### Props:

|Field|Required|Type/Default|Usage|
|-----|------|--------|-----|
|Columns|true|Array[Object]|The columns to be displayed with their descriptions and options|
|Rows|true|Array[Object]|The rows to be displayed, each row should have a key with the field from the columns.|
|tableClass|false|String / 'table is-bordered'|The class to be applied to the table.|
|click|false|Boolean / false|If rows (```<tr></tr>```) are clickable. On true it enables an emit method.|

##### Columns:
|Key|Required|Type/Default|Usage|
|---|---|---|---|
|label|false|String|The label to be shown in the table header and as the filter placeholder.|
|field|true|String|The field to print the rows with, it will also serve for the filter and sorting. It must match the rows' key.|
|sortable|false|Boolean / false|If true it enables sorting for the column.|
|filterable|false|Boolean / false|If true it enables filtering for the column.|
|headClass|false|String|Class added to the header `<th></th>` of the column|
|bodyClass|false|String|Class added to all the cells `<td></td>` that belong to the column.|
|width|false|String|The width of the column. Ex. '50px'|

#### Methods: 

* **row-click:** Called after a row was clicked `<tr></tr>` only if prop `click = true`.

## Examples:

### Simple:

![Simple Table](https://github.com/aislasq/vue-bulma-datatable/blob/master/images/simple.png "Simple Table")

```vue
<template>
    <div>
        <data-table :rows="rows"
                    :columns="columns"
                    :click="true"
                    tableClass="table is-striped is-bordered is-narrow"
                    @row-click="(val) => rowClick(val) ">
        </data-table>
    </div>
</template>

<script>
    import DataTable from '../../DataTable.vue';

    export default {
        components: {
            'data-table': DataTable
        },
        data: function(){
            return {
                columns: [
                    {
                        label: 'ID',
                        field: 'id'
                    },
                    {
                        label: 'Name',
                        field: 'name'
                    },
                    {
                        label: 'Paradigm',
                        field: 'paradigm'
                    },
                    {
                        label: 'Usage',
                        field: 'usage'
                    }
                ],
                rows: [
                    {id: 1, name: 'Java', paradigm: 'Object Oriented', usage: 0.227},
                    {id: 2, name: 'Haskell', paradigm: 'Functional', usage: 0.003},
                    {id: 3, name: 'C', paradigm: 'Procedural', usage: 0.066},
                    {id: 4, name: 'Scala', paradigm: 'Object Oriented / Functional', usage: 0.012},
                    {id: 5, name: 'Ruby', paradigm: 'Object Oriented', usage: 0.019},
                    {id: 6, name: 'Python', paradigm: 'Procedural / Object Oriented', usage: 0.161},
                    {id: 7, name: 'PHP', paradigm: 'Procedural / Object Oriented', usage: 0.093}
                ]
            }
        },
        methods: {
            rowClick: function(value){
                console.log(value);
            }
        }
    }
```

### Sortable

![Sortable Table](https://github.com/aislasq/vue-bulma-datatable/blob/master/images/sortable.png "Sortable Table")

```vue
<script>
    export default {
        data: function(){
            return {
                columns: [
                    {
                        label: 'ID',
                        field: 'id',
                        sortable: true
                    },
                    {
                        label: 'Name',
                        field: 'name'
                    },
                    {
                        label: 'Paradigm',
                        field: 'paradigm',
                        sortable: true
                    },
                    {
                        label: 'Usage',
                        field: 'usage',
                        sortable: true
                    }
                ]
            }
        }
    }
```

### Filterable:

![Filterable Table](https://github.com/aislasq/vue-bulma-datatable/blob/master/images/filterable.png "Filterable Table")

```vue
<script>
    export default {
        data: function(){
            return {
                columns: [
                    {
                        label: 'ID',
                        field: 'id',
                        sortable: true
                    },
                    {
                        label: 'Name',
                        field: 'name',
                        filterable: true
                    },
                    {
                        label: 'Paradigm',
                        field: 'paradigm',
                        sortable: true,
                        filterable: true
                    },
                    {
                        label: 'Usage',
                        field: 'usage',
                        sortable: true
                    }
                ]
            }
        }
    }
```
### Format cells:

![Formatted Table](https://github.com/aislasq/vue-bulma-datatable/blob/master/images/format.png "Formatted Table")

```vue
<template>
    <div>
        <data-table :rows="rows"
                    :columns="columns"
                    tableClass="table is-striped is-bordered is-narrow">
            <template slot="table-header-name">
                <th>Programming Language</th>
            </template>
            <template slot="table-body" scope="props">
                <td width="75px">{{ props.row.id }}</td>
                <td>{{ props.row.name }}</td>
                <td>{{ props.row.paradigm }}</td>
                <td width="100px" class="has-text-right">{{ Math.round(props.row.usage * 1000)/10 }} %</td>
            </template>
        </data-table>
    </div>
</template>
```

## Badges

![](https://img.shields.io/badge/license-MIT-blue.svg)
![](https://img.shields.io/badge/status-stable-green.svg)

