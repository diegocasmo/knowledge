# A Simple React ``<Tabs/>`` Component

A client of mine recently gave me the task to code a feature which included designing tabs similar to [those of Bootstrap](https://codepen.io/wizly/pen/BlKxo/). The client side of such project was built using React and I initially thought about using the [react-tabs](https://github.com/reactjs/react-tabs) library, but later on decided to implement it myself as it was a very simple requirement and I wanted to have complete control of how it worked.

The goal of this blog post is to share and explain the code I created to built such functionality. Thus, giving you an alternative if one day you need to create a similar component without having to include yet another dependency to your project.

### The Gist
I wanted to create a very flexible tabs component. The main goal was for it to allow any valid ``jsx`` to be rendered inside a tab. I wanted it to look something like this:

``` javascript
<Tabs>
    <Tab iconClassName={'icon-class-0'} linkClassName={'link-class-0'}>
        <p>content 0</p>
    </Tab>
    <Tab iconClassName={'icon-class-1'} linkClassName={'link-class-1'}>
        <CustomComponent propA={'foo'} propB={this.handleSomething}/>
    </Tab>
</Tabs>
```

As you can see, a tab should be able to render any valid ``jsx``. This gives the developer the ability to pass custom ``props`` for other functionality that might be needed in the future.

### The ``<Tabs/>`` component

The ``<Tabs/>`` component holds the state and  knows what tab is currently being rendered. It handles the changes to show/hide other tabs. Finally, it also gives the developer the ability to specify a default tab to render when none is selected.

> Note: A link to see all code with tests can be found at the end of this blog post.

``` javascript
export class Tabs extends Component {

    constructor(props, context) {
        super(props, context);
        this.state = {
            activeTabIndex: this.props.defaultActiveTabIndex
        };
        this.handleTabClick = this.handleTabClick.bind(this);
    }

    // Toggle currently active tab
    handleTabClick(tabIndex) {
        this.setState({
            activeTabIndex: tabIndex === this.state.activeTabIndex ? this.props.defaultActiveTabIndex : tabIndex
        });
    }

    // Encapsulate <Tabs/> component API as props for <Tab/> children
    renderChildrenWithTabsApiAsProps() {
        return React.Children.map(this.props.children, (child, index) => {
            return React.cloneElement(child, {
                onClick : this.handleTabClick,
                tabIndex: index,
                isActive: index === this.state.activeTabIndex
            });
        });
    }

    // Render current active tab content
    renderActiveTabContent() {
        const {children} = this.props;
        const {activeTabIndex} = this.state;
        if(children[activeTabIndex]) {
            return children[activeTabIndex].props.children;
        }
    }

    render() {
        return (
            <div className="tabs">
                <ul className="tabs-nav nav navbar-nav navbar-left">
                    {this.renderChildrenWithTabsApiAsProps()}
                </ul>
                <div className="tabs-active-content">
                    {this.renderActiveTabContent()}
                </div>
            </div>
        );
    }
};
```

### The ``<Tab/>`` component

The ``<Tab/>`` component is a stateless component. Its goal is to simply render a Bootstrap-like tab, allow the developer to specify some custom styling, and call its parent (the ``<Tabs/>`` component) when an ``onClick`` event occurs.

``` javascript
export const Tab = (props) => {
    return (
        <li className="tab">
            <a className={`tab-link ${props.linkClassName} ${props.isActive ? 'active' : ''}`}
                onClick={(event) => {
                    event.preventDefault();
                    props.onClick(props.tabIndex);
                }}>
                <i className={`tab-icon ${props.iconClassName}`}/>
            </a>
        </li>
    )
}
```

### Conclusion
With very little code and yet a lot of flexibility we have managed to implement a simple React ``<Tabs/>`` component. A gist with the source code needed to implement both components and their tests can be found [here](https://gist.github.com/diegocasmo/5cd978e9c5695aefca0c6a8a19fa4c69).

Have you ever implemented something similar? Did you do something differently? Leave a comment below a let's learn from each other :).
