# Front-end Coding Style

## Summary

1. [Commits](#commits)
1. [React](#react)
1. [CSS](#css) 
1. [JavaScript](#js)
1. [References](#references)

<a name="react"></a>
## 2. React

Our React Projects use [JavaScript Standard Style](https://github.com/standard/standard), but we have anothers rules for code organization with best practices.

## 2.1 Class Based Component
1. [Importing](#importing)
1. [Initializing State](#state)
1. [propTypes and defaultProps](#proptypes)
1. [mapStateToProps and mapDispatchToProps](#map) 
1. [Destructuring Props](#destructuring) 
1. [Methods](#methods)

<a name="importing"></a>
### 2.1.1 Importing

```javascript

/* Good */
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import { Link, withRouter } from 'react-router-dom'
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'
import { DatePicker, Input, Icon } from 'antd'

import moment from 'utils/momentUtils'
import ActivitiesMiddleware from 'modules/activities/middleware'
import Button from 'components/Ui/Buttons/ButtonDefault'
import PageHeader from 'components/Ui/PageHeader'

/* Bad */
import React, { Component } from 'react'
import { Link, withRouter } from 'react-router-dom'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { bindActionCreators } from 'redux'
import moment from 'utils/momentUtils'
import { DatePicker, Input, Icon } from 'antd'

import ActivitiesMiddleware from 'modules/activities/middleware'

import PageHeader from 'components/Ui/PageHeader'
import Button from 'components/Ui/Buttons/ButtonDefault'

```


<a name="state"></a>
### 2.1.2 Initializing State

```javascript
/* Good */
import React, { Component } from 'react'

class ActivityContainer extends Component {
  state = { expanded: false }
}

/* Bad */
import React, { Component } from 'react'

class ActivityContainer extends Component {
  constructor (props) {
    super(props)
    this.state = { expanded: true }
  }
}

```

<a name="proptypes"></a>
### 2.1.3 propTypes and defaultProps

PropTypes should be defined only on dumb components!

```javascript

/* Good */
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class ActivityContainer extends Component {
  state = { expanded: false }
 
  static propTypes = {
    model: PropTypes.object.isRequired,
    title: PropTypes.string
  }
 
  static defaultProps = {
    model: {
      id: 0
    },
    title: 'Your Name'
  }
}
  
/* Bad */
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class ActivityContainer extends Component {
  state = { expanded: false }
}

ActivityContainer.propTypes = {
  model: PropTypes.object.isRequired,
  title: PropTypes.string
}
 
ActivityContainer.defaultProps = {
  model: {
    id: 0
  },
  title: 'Your Name'
}
 
```

<a name="map"></a>
### 2.1.4 mapStateToProps and mapDispatchToProps

```javascript

import React, { Component } from 'react'
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'

import ActivityMiddleware from 'modules/activity/middleware'

const mapStateToProps = ({ activity }) => ({
  activity: activity.item,
})

const mapDispatchToProps = (dispatch) => (
  bindActionCreators({
    fetchActivityItemById: (id) => ActivityMiddleware.fetchActivityItemById(id),
  }, dispatch)
)

export default connect(mapStateToProps, mapDispatchToProps)(ActivityContainer)

```

<a name="destructuring"></a>
### 2.1.5 Destructuring Props

```javascript
/* Good */

const {
  model,
  title
} = this.props

<ExpandableForm 
  onSubmit={this.handleSubmit} 
  expanded={this.state.expanded} 
  onExpand={this.handleExpand}>
</ExpandableForm>

/* bad */

const { model, title } = this.props

<ExpandableForm  onSubmit={this.handleSubmit} expanded={this.state.expanded} 
  onExpand={this.handleExpand}>
</ExpandableForm>

```

<a name="methods"></a>
### 2.1.6 Methods

```javascript
/* Good */
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'

import ActivityMiddleware from 'modules/activity/middleware'
import ActivityForm from './form/ActivityForm'

class ActivityContainer extends Component {
  state = { expanded: false }
 
  static propTypes = {
    model: object.isRequired,
    title: string
  }
 
  static defaultProps = {
    model: {
      id: 0
    },
    title: 'Your Name'
  }
  
  rennder () {
    return <ActivityForm
      handleSubmit={this.handleSubmit}
      handleNameChange={this.handleNameChange}
      handleExpand={this.handleExpand}
    />
  }
  
  handleSubmit = (e) => {
    e.preventDefault()
    this.props.model.save()
  }
  
  handleNameChange = (e) => {
    this.props.model.changeName(e.target.value)
  }
  
  handleExpand = (e) => {
    e.preventDefault()
    this.setState({ expanded: !this.state.expanded })
  }
}

const mapStateToProps = ({ activity }) => ({
  activity: activity.item,
})

const mapDispatchToProps = (dispatch) => (
  bindActionCreators({
    fetchActivityItemById: (id) => ActivityMiddleware.fetchActivityItemById(id),
  }, dispatch)
)

export default connect(mapStateToProps, mapDispatchToProps)(ActivityContainer)

/* Bad */
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'

import ActivityMiddleware from 'modules/activity/middleware'
import ActivityForm from './form/ActivityForm'

const mapStateToProps = ({ activity }) => ({
  activity: activity.item,
})

const mapDispatchToProps = (dispatch) => (
  bindActionCreators({
    fetchActivityItemById: (id) => ActivityMiddleware.fetchActivityItemById(id),
  }, dispatch)
)

class ActivityContainer extends Component {
  state = { expanded: false }
 
  static propTypes = {
    model: object.isRequired,
    title: string
  }
 
  static defaultProps = {
    model: {
      id: 0
    },
    title: 'Your Name'
  }
  
  handleSubmit = (e) => {
    e.preventDefault()
    this.props.model.save()
  }
  
  handleNameChange = (e) => {
    this.props.model.changeName(e.target.value)
  }
  
  handleExpand = (e) => {
    e.preventDefault()
    this.setState({ expanded: !this.state.expanded })
  }
  
  rennder () {
    return <ActivityForm
      handleSubmit={this.handleSubmit}
      handleNameChange={this.handleNameChange}
      handleExpand={this.handleExpand}
    />
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(ActivityContainer)

```


<a name="css"></a>
## 3. CSS

The main influences for the CSS rules are [Code Guide by @mdo](https://github.com/mdo/code-guide) and [idiomatic CSS](https://github.com/necolas/idiomatic-css/).

### CSS Summary

1. [CSS Syntax](#css-syntax)
1. [CSS Declaration Order](#css-order)
1. [CSS Class Name](#css-class-name)
1. [CSS Units](#css-units)
1. [CSS Performance](#css-performance)
1. [CSS Media Queries](#css-media-queries) 

<a name="css-syntax"></a>
### 3.1. CSS Syntax

Use soft tabs with two spaces. You can configure your editor for this.

```css
/* Good */
.nav-item {
  display: inline-block;
  margin: 0 5px;
}

/* Bad */
.nav-item {
    display: inline-block;
    margin: 0 5px;
}
```

Always use double quotes.

```css
/* Good */
[type="text"]
[class^="..."]

.nav-item:after {
  content: "";
}

/* Bad */
[type='text']
[class^='...']

.nav-item:after {
  content: '';
}
```

Include a single space before the opening bracket of a ruleset.

```css
/* Good */
.header {
  ...
}

/* Bad */
.header{
  ...
}
```

Include a single space after the colon of a declaration.

```css
/* Good */
.header {
  margin-bottom: 20px;
}

/* Bad */
.header{
  margin-bottom:20px;
}
```

Include a semi-colon at the end of the last declaration in a declaration block.

```css
/* Good */
.header {
  margin-bottom: 20px;
}

/* Bad */
.header{
  margin-bottom:20px
}
```

Keep one declaration per line.

```css
/* Good */
.selector-1,
.selector-2,
.selector-3 {
  ...
}

/* Bad */
.selector-1, .selector-2, .selector-3 {
  ...
}
```

Single declarations should remain in one line.

```css
/* Good */
.selector-1 { width: 50%; }

/* Bad */
.selector-1 {
  width: 50%;
}
```

Separate each ruleset by a blank line.

```css
/* Good */
.selector-1 {
  ...
}

.selector-2 {
  ...
}

/* Bad */
.selector-1 {
  ...
}
.selector-2 {
  ...
}
```

Use lowercase, shorthand hex values and avoid specifying units is zero-values.

```css
/* Good */
.selector-1 {
  color: #aaa;
  margin: 0;
}

/* Bad */
.selector-1 {
  color: #AAAAAA;
  margin: 0px;
}
```

<a name="css-order"></a>
### 3.2. CSS Declaration Order

The declarations should be added in alphabetical order.

```css
/* Good */
.selector-1 {
  background: #fff;
  border: #333 solid 1px;
  color: #333;
  display: block;
  height: 200px;
  margin: 5px;
  padding: 5px;
  width: 200px;
}

/* Bad */
.selector-1 {
  padding: 5px;
  height: 200px;
  background: #fff;
  margin: 5px;
  width: 200px;
  color: #333;
  border: #333 solid 1px;
  display: block;
}
```

<a name="css-class-name"></a>
### 3.3. CSS Class Name

Keep class lowercase and use dashes to separate the classname.

```css
/* Good */
.page-header { ... }

/* Bad */
.pageHeader { ... }
.page_header { ... }
```

Use single dash to element name, double underline to element block and double dash to style modification.

```css
/* Good */
.page-header__action { ... }
.page-header__action__title { ... }
.page-header--active { ... }

.btn { ... }
.btn--primary { ... }

/* Bad */
.page-header-action { ... }
.page-header-action-title { ... }
.page-header-active { ... }

.btn { ... }
.btn-primary { ... }
```

Dashes and underline serve as natural breaks in related class. Prefix class based on the closest parent or base class.

```css
/* Good */ 
.nav { ... }
.nav__item { ... }
.nav__link { ... }

/* Bad */
.item-nav { ... }
.link-nav { ... }
```

Avoid giving too short names for class and always choose meaningful names that provide the class function.

```css
/* Good */
.btn { ... }
.page-header { ... }
.progress-bar { ... }

/* Bad */
.s { ... }
.ph { ... }
.block { ... }
```

<a name="css-performance"></a>
### 3.4. Units

Pixels are ignorant, don’t use them.
Use REMs for sizes and spacing.
Use EMs for media queries.

```css
/* Good */
.btn {
  font-size: 1.6rem;
  padding: .5rem;
}

/* Bad */
.btn {
  font-size: 16px;
  padding: 5px;
}
```

<a name="css-performance"></a>
### 3.5. CSS Performance

Never use IDs.

```css
/* Good */
.header { ... }
.section { ... }

/* Bad */
#header { ... }
#section { ... }
```

Do not use selectors standards for not generic rules, always preferably for class.

```css
/* Good */
.form-control { ... }
.header { ... }
.section { ... }

/* Bad */
input[type="text"] { ... }
header
section
```

Avoid nesting elements, the preference is always to use class.

```css
/* Good */
.navbar { ... }
.nav { ... }
.nav__item { ... }
.nav__link { ... }

/* Bad */
.navbar ul { ... }
.navbar ul li { ... }
.navbar ul li a { ... }
```

Nest only when need change the class comportament with interference for other class. Keep the nested on max of three elements.

```css
/* Good */
.modal-footer .btn { ... }
.progress.active .progress-bar { ... }

/* Bad */
.modal-btn { ... }
.progress.active .progress-bar .progress-item span { ... }
```

Always minify the CSS code. [Webpack](https://webpack.js.org) leaves this easier. In [Webpack 4](https://github.com/webpack/webpack/releases) make default with 0 configuration.

```css
<!-- Good -->
.navbar { ... }.nav { ... }.nav-item { ... }.nav-link { ... }

<!-- Bad -->
.nav-item {
  ...
}
.nav-link {
  ...
}
```

<a name="css-media-queries"></a>
### 3.6 CSS Media Queries

Start the development with generic rules with and add media queries with mobile first.

```css
/* Good */
.navbar {
  margin-bottom: 20px;
}

@media (min-width: 480px) {
  .navbar {
    padding: 10px;
  }
}

@media (min-width: 768px) {
  .navbar {
    position: absolute;
    top: 0;
    left: 0;
  }
}

@media (min-width: 992px) {
  .navbar {
    position: fixed;
  }
}

/* Bad */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
}

@media (max-width: 767px) {
  .navbar {
    position: static;
    padding: 10px;
  }
}

```

Keep the media queries as close to their relevant rule sets whenever possible. Don't bundle them all in a separate stylesheet or at the end of the document.

```css
.navbar { ... }
.nav { ... }
.nav-item { ... }

@media (min-width: 480px) {
  .navbar { ... }
  .nav { ... }
  .nav-item { ... }
}
```
