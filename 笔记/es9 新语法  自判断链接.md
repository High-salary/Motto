```
const car = {
    model: 'Fiesta',
    manufacturer: {
        name: 'Ford',
        address: {
            street: 'Some Street Name',
            number: '5555',
            state: 'USA'
        }
    }
}
const model = car && car.model || 'default model';
const model = car ?.model ?? 'default model';//新语法 还不支持

const street = car && car.manufacturer && car.manufacturer.address && car.manufacturer.address.street || 'defaultstreet';
const street = car ?.manufacturer ?.address ?.street ?? 'default street';//新语法 还不支持

const value = document.querySelector('input#user-name') ? .value;//新语法 还不支持
```