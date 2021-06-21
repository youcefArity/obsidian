Readme
const useStyles = makeStyles(theme => ({
  appBar: {
    boxShadow: '0 3px 10px 0 rgba(57,58,60,0.1)'
  },

  bottomAppBar: {
    marginTop: '10px',
    top: 'auto',
    bottom: 0
  },

  bottomAppBarSm: {
    marginTop: '10px',
    top: 'auto',
    bottom: 0,
    height: 70
  },

  menuButtom: {
    textTransform: 'inherit',
    height: 70,
    wordWrap: 'break-word',
    padding: 5
  },

  topLogo: {
    padding: 20,
    marging: '5px',
    overflow: 'visible',
    [theme.breakpoints.down('sm')]: {
      paddingLeft: '0 !important',
      padding: '20px'
    }
  },

  topLogoDiscover: {
    marginTop: '25px',
    [theme.breakpoints.down('sm')]: {
        marginTop: '25px',
        marginLeft: '20px'
    }
  },

  topLogoDiscoverButton: {
    borderRadius: '5px', 
    boxShadow: 'none',
    [theme.breakpoints.down('sm')]: {
      fontSize: '10px'
    },
    [theme.breakpoints.down('xs')]: {
      fontSize: '8px'
    }
  },

  topConnected: {
  },

  layoutYield: {
    [theme.breakpoints.down('sm')]: {
      paddingBottom: '80px'
    }
  },

  logo: {
    height: '50px',
    lineHeight: 0,
    overflow: 'visible',
    [theme.breakpoints.down('sm')]: {
      height: '30px'
    }
  },

  container: {
    minHeight: 'calc(100vh - 165px)',
    [theme.breakpoints.down('sm')]: {
      minHeight: 'calc(100vh - 55px)',
    },
    padding: '10px 0 10px 0'
  },

  childrenLayout: {
    width: '850px',
    paddingTop: '32px',
    paddingBottom: '32px',
    [theme.breakpoints.down('sm')]: {
      paddingTop: '15px',
      paddingBottom: '70px',
      width: '100%'
    }
  },

  forbidHorizontalScroll: {
    [theme.breakpoints.down('sm')]: {
      width: '100vw',
      overflowX: 'hidden'
    }
  }
}))

const StagingMessage = () =>
  <Snackbar
    anchorOrigin={{
      vertical: 'top',
      horizontal: 'right'
    }}
    style={{ maxWidth: '100px', top: 100 }}
    open
    autoHideDuration={6000}
  >
    <SnackbarContent
      style={{ backgroundColor: amber[600] }}
      message={
        <Typography>
          Ceci est notre plateforme de test
        </Typography>
      }
    />
  </Snackbar>

export default withAuth( 
withRouter(({ children, appendToHeader, auth, router }) => {
  const store = createStore(rootReducer)
  store.subscribe(() => {
    saveState({
      ...store.getState()
    })
  })
  const classes = useStyles()
  const [isMonoprixClient, setIsMonoprixClient] = React.useState(false)

  const canShowMonoprixLogo = auth.isLoggedIn() && (
    auth.user().canShowHomepage()
    || auth.user().bloodTestsStatus === "controlWaiting"
    || auth.user().bloodTestsStatus === "controlDownloaded"
    || auth.user().bloodTestsStatus === "controlDone"
    || auth.user().bloodTestsStatus === "controlAccepting"
    || auth.user().bloodTestsStatus === "controlAccepted"
    || auth.user().bloodTestsStatus === "controlReceived"
    || auth.user().bloodTestsStatus === "controlCompleted")
  

  React.useEffect(() => {
    auth.fetch('Clients/me/brands').then((brands) => {
      if (brands.filter(brand => brand.name === 'Monoprix').length > 0 && router.pathname !== '/commandez')
        setIsMonoprixClient(true)
      else
        setIsMonoprixClient(false)
    }).catch((err) => {
      console.log(err)
    })
  }, [])

  const ConditionalWrapper = ({ condition, children }) => condition ? <Provider store={store}>{children}</Provider> : children;
  return (
    <ConditionalWrapper condition={!['/TestMe', '/questionnaire', '/', '/changements', '/bilan', '/change/show', '/biomarker/show'].includes(router.pathname)}>
      <div className={classes.forbidHorizontalScroll}>
        <Box displayPrint="none">
          {showStagingMessage && <StagingMessage />}

          <AppBar color="inherit" position="static" className={classes.appBar}>
            <Container>
              <Toolbar>
                <Grid container>
                  <Grid item xs={4} md={3} className={classes.topLogo}>
                  {(environmentIdentifier !== 'seo' && typeof location !== 'undefined' && location.pathname !== '/freemium_questionnaire') ?
                    <Link href="/">
                        <img src={Logo} alt="logo" className={classes.logo}/>
                    </Link> 
                    : 
                    <Link href={wordpressDomain}>
                      <img src={Logo} alt="logo" className={classes.logo} />
                    </Link> 
                  }
                  </Grid>

                  <Grid item xs={5} md={isMonoprixClient ? 5 : 6}>
                    <Hidden smDown>
                      <div style={{ marginTop: 10 }}>
                        <AppMenu activeButtonVariant="text" />
                      </div>
                    </Hidden>

                    { environmentIdentifier === 'seo' && 
                      <Typography align="center" className={classes.topLogoDiscover}>
                        <A href={wordpressDomain}><Button color="primary" variant="contained" className={classes.topLogoDiscoverButton}>Découvrez 1Food1me</Button></A>
                      </Typography> 
                    }
                    <FreemiumOrderButton />
                  </Grid>
                  
                  {canShowMonoprixLogo && isMonoprixClient && <MonoprixPanier />}
                  <Grid item xs={3} md={isMonoprixClient ? 2 : 3} style={{ display: 'flex' }} className={classes.topConnected}>
                    <Typography style={{ display: 'flex', flex: 1, justifyContent: 'flex-end', alignItems: 'center' }} component="div">
                      <NoSSR />
                    </Typography>
                  </Grid>
                </Grid>
              </Toolbar>
            </Container>
          </AppBar>
          {appendToHeader && appendToHeader()}
        </Box>

        <Grid container className={classes.container} alignContent="center" alignItems="center" justify="center" direction="column">
          <Grid item className={classes.layoutContainer}>
            <NoSsr>
              <Container className={classes.childrenLayout}>
                {children}
              </Container>
            </NoSsr>
          </Grid>
        </Grid>

        <Hidden mdUp>
          <AppBar position="fixed" color="inherit" className={classes.bottomAppBarSm}>
            <AppMenu activeButtonVariant="text" />
          </AppBar>
        </Hidden>

        <Hidden smDown>
          <Box displayPrint="none">
            <AppBar position="static" color="inherit" className={classes.bottomAppBar}>
              <Container>
                <Toolbar>
                  <Grid container>
                    <Grid item xs={6}>
                      <Typography variant="subtitle2" style={{ margin: '10px' }}>
                        Contactez-nous : hello@1food1me.com
                      </Typography>
                    </Grid>
                    <Grid item xs={6}>
                      <Typography variant="subtitle2" align="right" style={{ margin: '10px' }}>
                        © 2019-2020 1Food1Me
                      </Typography>
                    </Grid>
                  </Grid>
                </Toolbar>
              </Container>
            </AppBar>
          </Box>
        </Hidden>
      </div>
    </ConditionalWrapper>
  )
}))

@withRouter
class MonoprixPanier extends Component {
  
  render() {
    const { router } = this.props
    return (
      (router.pathname !== '/commandez') &&
      <Grid item xs={2} md={2} style={{maxWidth: 150, marginTop: 4}} container direction="column" align="center" justify="center">
        <a
          style={{ 
            textDecoration: 'none',
            color: '#7D7B8F'
          }}
          href='https://courses.monoprix.fr/basket?utm_source=1food1me&utm_campaign=1food1me_poc&utm_medium=web&utm_content=basket'
          target='_blank'
          >
          <Grid item>
              <img src={logoMonoprix} style={{ color: 'blue' }} width="20%"/>
            </Grid>
            <Grid item >
              <Typography style={{ fontSize: '14px', fontWeight: '500' }} color="#7D7B8F" align="center">Mon panier de courses Monoprix</Typography>
            </Grid>
          </a>
      </Grid>
    )
  }
}

const NoSSR = dynamic(
  () => import('./menu'),
  {
    loading: () => null,
    ssr: false })


const ActiveLinkButton = withStyles({
  label: {
    display: 'flex',
    flexDirection: 'column',
    fontSize: 'rgba(125,123,139,1)'
  }
})(Button)


export const ActiveLink = ({ href, label, activeButtonVariant, icon, disabled }) => {
  const classes = useStyles()
  let isActive = (typeof window === 'object') && location.pathname.startsWith(href) && (href === location.pathname || href !== '/')

  const activeLinkButton = (
    <ActiveLinkButton color={isActive ? 'primary' : 'inherit'} variant={isActive ? activeButtonVariant : 'text'} className={classes.menuButtom} disabled={disabled}>
      <span>{icon && <img src={icon} alt="" style={{ height: '30px' }} />}</span>
      { environmentIdentifier !== 'seo' ? 
        <span>{label}</span>
        :
        <span style={{color: '#7D7B8B'}}>{label}</span>
      }
    </ActiveLinkButton>
  )

  // only case where disabled is true is on the post consultation page, so we can hard code value of tooltipText for now.
  return disabled ?
    <ButtonWithTooltip style={{ textTransform: 'inherit', padding: 0 }} tooltipText="Commandez le programme 1Food1Me et activez un parcours nutritionnel unique fondé sur votre biologie" disabled={disabled} >
        {activeLinkButton}
    </ButtonWithTooltip>
    :
    <Link href={href} prefetch={true}>
      {activeLinkButton}
    </Link>
}

@connect(({ client }) => ({ client }))
@withRouter
@withAuth
@withFetch(({ fetch }) => ({ products: fetch('/products/list') }))
class AppMenu extends React.Component {
  shouldComponentUpdate(nextProps) {
    return nextProps.client.sale !== this.props.client.sale
  }
  render () {
    const { auth, userArchivedBiomarkers } = this.props
    if (!auth.isLoggedIn() || ['/questionnaire', '/commandez'].includes(this.props.router.route) || !this.props.auth.user().sale && !this.props.auth.user().freemium) return null

    const { activeButtonVariant } = this.props
    const isControlStep = auth.user().bloodTestsStatus === "controlWaiting" 
    || auth.user().bloodTestsStatus === "controlDownloaded"
    || auth.user().bloodTestsStatus === "controlDone"
    || auth.user().bloodTestsStatus === "controlAccepting"
    || auth.user().bloodTestsStatus === "controlAccepted"
    || auth.user().bloodTestsStatus === "controlReceived"
    || auth.user().bloodTestsStatus === "controlCompleted"

    return (
      <Grid container direction="row" style={{ flex: 1, flexWrap: 'nowrap' }} justify="space-evenly">
        <Grid item>
          {(((auth.user().canShowHomepage() || isControlStep) && environmentIdentifier !== 'seo') || auth.user().consultationStatus === 'completed') &&
            <ActiveLink href="/" label="Accueil" activeButtonVariant={activeButtonVariant} icon={HomeIcon}  />
          }
          {environmentIdentifier === 'seo' && 
            <Hidden mdUp>
            <A href={wordpressDomain}>
              <ActiveLink href={wordpressDomain} isActive={false} label="Accueil" activeButtonVariant={activeButtonVariant} icon={HomeIcon} />          
            </A>
            </Hidden>
          }
        </Grid>
        <Grid item>
          {(((auth.user().canShowHomepage() || isControlStep)) || auth.user().consultationStatus === 'completed') &&
            <ActiveLink href="/changements" label="Changements" activeButtonVariant={activeButtonVariant} icon={ChangeIcon} disabled={this._isMenuBtnDisabled()} />
          }
        </Grid>
        <Grid item>
          {(((auth.user().canShowHomepage() || isControlStep)) || auth.user().consultationStatus === 'completed') &&
            <ActiveLink href="/bilan" label={auth.user().bloodTestsStatus.includes("control") ? "Bilans" : "Bilan"} activeButtonVariant={activeButtonVariant} icon={BilanIcon} disabled={this._isMenuBtnDisabled()} />
          }
        </Grid>
        <Grid item>
          {((((auth.user().canShowHomepage() || isControlStep) && environmentIdentifier !== 'seo')) || auth.user().consultationStatus === 'completed') &&
            <ActiveLink href="/menus" label="Menus" activeButtonVariant={activeButtonVariant} icon={MenuIcon} disabled={this._isMenuBtnDisabled()} />
          }
          { environmentIdentifier === 'seo' &&
            <Hidden mdUp>
              <ActiveLink href="/connexion" label="Connexion" activeButtonVariant={activeButtonVariant} icon={ProfileIcon} />
            </Hidden>
          }
        </Grid>
      </Grid>
    )
  }
  _isMenuBtnDisabled = () =>  {
    const { auth, products } = this.props;
    if(!auth.user().sale) return true
    const productId = auth.user().sale.productId 
    const sku = products.find(p => p.id === productId).sku
    return sku === '1F1M000N' && auth.user().consultationStatus === 'completed'
  }
}

@withStyles((theme) => ({
  topLogoDiscover: {
    marginTop: '15px',
    [theme.breakpoints.down('sm')]: {
        marginTop: '5px',
        marginLeft: '20px'
    }
  },
  topLogoDiscoverButton: {
    borderRadius: '5px', 
    boxShadow: 'none',
    [theme.breakpoints.down('sm')]: {
      fontSize: '10px'
    },
    [theme.breakpoints.down('xs')]: {
      padding: 5,
      fontSize: '8px'
    }
  },
  purchase: {
    [theme.breakpoints.down('sm')]: {
      fontSize: '10px',
      padding: '6px 8px'
    },
    [theme.breakpoints.down('xs')]: {
      fontSize: '8px',
      padding: '7px 5px'
    }
  }
}))
@withRouter
@withAuth
class FreemiumOrderButton extends React.Component {

  state = {
    hasSale: true
  }

  constructor(props) {
    super(props)
  }
  componentDidMount() {
    const {auth: { fetch }} = this.props
      fetch(`Clients/me/recentSale`).then((sale) => {
        if(!sale){
          this.setState({
            hasSale: false
          })
        }
      }).catch((err) => {
        console.log(err);
      })
  }
  render () {
    const { auth, router, classes } = this.props

    return (
      !(router.pathname === '/freemium_questionnaire' || router.pathname === '/commandez' || router.pathname.includes('/profile/mot-de-passe')) &&

      <div>
        {
          (!auth.isLoggedIn() || (auth.isLoggedIn() && auth.user().freemium && !this.state.hasSale) || environmentIdentifier === 'seo')
            ? (
              <Typography align="center" style={{ marginTop: 15}} className={classes.purchase}>
                <A href={`${wordpressDomain}/programme?utm_source=1food1me&utm_medium=webapp&utm_content=freemium_questionnaire_pages`}>
                  <Button color="primary" variant="contained" className={classes.purchase}>Commandez 1Food1me</Button>
                </A>
              </Typography>
            )
            : (
              <div></div>
            )
        }
      </div> 
    )  
  }
}

import React, { Children } from 'react';
import Document, { Head, Main, NextScript } from 'next/document';
import { ServerStyleSheets } from '@material-ui/styles'
import withAuth from '../../context/with_auth'
import flush from 'styled-jsx/server'
import { AppBar, Container, Toolbar, Menu, MenuItem, Typography, Grid, Hidden, Button, Box, NoSsr, Snackbar, SnackbarContent, Link as A } from '@material-ui/core'
import LockIcon from '@material-ui/icons/Lock'
import { makeStyles, withStyles } from '@material-ui/core/styles'
import { withRouter } from 'next/router'
import dynamic from 'next/dynamic'
import Link from 'next/link'
import { amber } from '@material-ui/core/colors'

import HomeIcon from '../../assets/icon-accueil.svg'
import BilanIcon from '../../assets/icon-bilan.svg'
import ChangeIcon from '../../assets/icon-changement.svg'
import MenuIcon from '../../assets/icon-menu.svg'
import ProfileIcon from '../../assets/icon-profil.svg'
import Logo from '../../../static/images/logo.svg'
import { showStagingMessage } from '../../../env'
import { environmentIdentifier, wordpressDomain } from '../../../env'
import sign_in from './sign_in'
import logoMonoprix from '../../../static/images/monoprix/logoMonoprixRouge.svg'
import { Component } from 'react'
import { ButtonWithTooltip } from '../../../src/shared/disablable_button_tooltip'
import { withFetch } from '../../services/with_fetch'
import { connect, Provider } from 'react-redux'
import { createStore } from "redux"
import { rootReducer, saveState } from '../../../pages/commandez'

