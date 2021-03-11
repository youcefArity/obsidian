```javascript
import { Button, Typography } from '@material-ui/core'

import React, { Component } from 'react'

import { Loading } from '../utils/loading'

import PropTypes from 'prop-types'

import { environmentIdentifier } from '../../env'

  
  

/\*

 \* This is a HOC (Higher Order Component) function that streamlines API fetching needs of components.

 \* It handles loading state, error displaying and data fetching and passing to wrapped component

 \* The function takes one parameter, which is a function that should return a object of the following form:

 \* { questions: fetch('questions'), ... }. The key 'question' is the name of the props that will be passed to the

 \* wrapped component. The value (fetch('questions')) is any promise that this HOC will wait to and catch

 \*/

// eslint-disable-next-line max-lines-per-function

export const withFetch = (loadData) \=> FetchableComponent \=> (

 class extends Component {

 constructor (props) {

 if (!props.auth) throw new Error('You should use the @withAuth decorator before the @withFetch one')

  

 super(props)

 }

  

 state = {

 error: null,

 hasError: false,

 loading: true

 }

  

 componentDidMount () {

 this.loadData()

 }

  

 loadData (showLoading = true) {

 if (showLoading) {

 this.setState({ loading: true })

 }

  

 const requests = loadData.call(this, {

 fetch: this.props.auth.fetch,

 props: this.props })

  

 Object.entries(requests).forEach((\[stateKey, promise\]) \=> {

 promise

 .then((res) \=> this.setState({ \[stateKey\]: res }))

 .catch((error) \=> this.setState({ error, hasError: true, loading: false }))

 })

  

 Promise

 .all(Object.values(requests))

 .then(() \=> this.setState({ hasError: false, loading: false }))

 .catch((error) \=> this.setState({ error, hasError: true, loading: false }))

 }

  

 render () {

 const { loading, hasError, error, ...data } = this.state

  

 const reloadData = this.loadData.bind(this)

  

 if (loading && !this.props.disableLoading) {

 return <Loading />

 }

  

 if (hasError) {

 return (

 <div\>

 <Typography align\="center" component\="div"\>

 <Typography variant\="h2"\>Mince !</Typography\>

  

 <Typography\>

 <img src\="/static/images/no-internet.png" alt\="" style\={{ width: '200px' }} />

 </Typography\>

  

 <Typography paragraph\={true}\>

 Une erreur s'est produite. Veuillez vérifier votre accès Internet puis :

 </Typography\>

  

 <Typography\>

 <Button variant\="outlined" onClick\={reloadData}\>Réessayez</Button\>

 </Typography\>

 </Typography\>

 </div\>

 )

 }

  

 return (

 <FetchableComponent {...this.props} {...data} reloadData\={reloadData} />

 )

 }

 }

)

  

// Add this to your component proptypes static propTypes = { ...withFetchProptypes }

export const withFetchProptypes = {

 reloadData: PropTypes.func.isRequired

}
```


```javascript

 componentDidMount () {

 window.scrollTo(0, 0)

  

 const { router: { query }, previousStep, nextStep, updateDispatcher, sale, auth, user: {firstName}, loadedLabs } = this.props

  

 if(query.isGift === true) {

 updateDispatcher(\['sale', 'isGift'\], query.isGift)

 updateDispatcher(\['sale', 'giftEmail'\], query.giftEmail)

 updateDispatcher(\['sale', 'giftDate'\], query.giftDate)

 this.setState({datepickGift: new Date(sale.giftDate)})

 }

 if(sale.isGift) {

 updateDispatcher(\['sale', 'giftMessage'\], sale.giftMessage.replace('{name}', firstName))

 }

 updateDispatcher(\['labs'\], loadedLabs.labs || \[\])

 if (query.session\_id) {

 // TODO: important check with the help of the stripe API that the session\_id returned is valid --> this is done in the API code

 if(query.sku === '1F1M0002'){

 updateDispatcher(\['step'\], 5)

 Promise.all(\[

 auth.fetch(\`clients/me/allSales\`, {

 method: 'POST',

 body: {

 ...sale,

 sku: '1F1M0002',

 stripeToken: "checkout\_payment",

 stripeChargeId: query.session\_id

 }

 }),

 auth.fetch(\`clients/me\`, {

 method: 'PATCH',

 body: { step: 5 , bloodTestsStatus: 'controlWaiting' }

 })

 \]).then(() \=> {

 updateDispatcher(\['step'\], 5)

 }

 ).catch(() \=> {

 updateDispatcher(\['step'\], 3) // in case of fail go back to step 4

 })

 }else{

 Promise.all(\[

 auth.fetch(\`clients/me/allSales\`, {

 method: 'POST',

 body: {

 ...sale,

 stripeToken: "checkout\_payment",

 stripeChargeId: query.session\_id

 }

 }),

 auth.fetch(\`clients/me\`, {

 method: 'PATCH',

 body: { step: 5 , algorithmComputed: false, triggeredAt: null, algoChangeGroups: \[\], algoChangeScores: \[\]}

 })

 \]).then(() \=> {

 updateDispatcher(\['step'\], 5)

 }

 )

 } 

 }

 }

```