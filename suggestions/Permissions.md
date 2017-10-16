# Data permissions

Patterns of entity permissions allows to simplify client side logic.

```json
{
    data: {
        id: 1,
        name: "John Dou",
        status: "active",
        messages: 123
    },
    permissions: {
        edit: true,
        delete: false
    }
}
```

Permissions make templates simple and elegant.
```jsx
<label>{data.name}</label>
<button disabled={permissions.edit}>Edit</button>
```

Same thing for entity lists. You have list permissions (create and bulk operations) and entity level permissions.

```json
{
    data: [
        {
            data: {
                id: 1,
                name: "John Dou",
                status: "active",
                messages: 123
            },
            permissions: {
                edit: true,
                delete: false
            }
        },{
            data: {
                id: 2,
                name: "Jim Bim"
                status: "active",
                messages: 12
            },
            permissions: {
                edit: false,
                delete: false
            }
        }
    ],
    permissions: {
        create: true,
        bulkDelete: false
    }
}
```


