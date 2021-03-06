# React component pattern

Use this pattern for the top level components with Redux state. This component sync search field with server and show results as simple bullet list.

```typescript
import { StateSync } from '@state-sync/js-client';
import * as React from 'react';
import { connect, Dispatch } from 'react-redux';
import { AREA_TEST, TestState, ResultItem } from '../../store/test';

// get area reference
const area = StateSync().area(AREA_TEST);

// declare component props
interface Props {
    data: TestState;
}

// declare component actions
interface Actions {
    bindSearch: React.ChangeEventHandler<HTMLInputElement>;
}

// Builder of component props
const PropsBuilder = (state: State): Props => {
    return {
        data: state.data
    };
};

// Builder of component actions
const ActionsBuilder = (dispatch: Dispatch<State>): Actions => {
    return {
        bindSearch: SyncStateBind.bind(area, '/search'),
    };
};

// Define component
class Component extends React.Component<Props & Actions> {

    componentDidMount() {
        // subscribe area
        area.subscribe();
    }

    componentWillUnmount() {
        // unsubscribe area
        area.unsubscribe();
    }
    
    render() {
        const {data, bindSearch} = this.props;
        return (
            <Input value={data.search} onChange={bindSearch}/>
            <ul>{data.results.map((item: ResultItem) => (<li>{result.text}</li>)}</ul>
        );
    }
}

// connect to Redux
const WrappedComponent = connect<Model, Actions>(PropsBuilder, ActionsBuilder)(Component);

// export actual component
export default WrappedComponent;
```
