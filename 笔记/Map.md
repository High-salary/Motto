```
const cars = new Map()
    .set('usa', () => { console.log(this); })
    .set('france', () => { console.log(222); })
    .set('italy', () => {
        console.log(1111123333)
    });

const getCarsByState = (state) => {
    cars.get(state) && cars.get(state)();
}

console.log(getCarsByState());
console.log(getCarsByState('usa'));
console.log(getCarsByState('italy'));


const reducer = new Map()
    .set('use', (state, action) => 1)
    .set('name', (state, action) => 1)

const getCarsByState = (state, action) => {
    reducer.get(action.type) && cars.get(action.type)(state, action)
}
```