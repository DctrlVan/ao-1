<template lang='pug'>

#wrex
    .intro(v-if='!$store.getters.isLoggedIn')
        .row
            .six.columns.sidewalk
                shared-title(title='sidewalk controls')
                button(@click='trySidewalk("crazy")').redwx crazy
                button(@click='trySidewalk("chill")').purplewx chill
                button(@click='trySidewalk("dna")').greenwx dna
                input(v-model='banner')
                button(@click='trySidewalk("banner")').bluewx banner
                button(@click='trySidewalk("rainbow")').yellowwx rainbow
                .row
                    .two.grid
                        button(@click='trySidewalk("color", "red")').redwx
                    .two.grid
                        button(@click='trySidewalk("color", "purple")').purplewx
                    .two.grid
                        button(@click='trySidewalk("color", "blue")').bluewx
                    .two.grid
                        button(@click='trySidewalk("color", "green")').greenwx
                    .two.grid
                        button(@click='trySidewalk("color", "yellow")').yellowwx
                    .two.grid
                        button(@click='trySidewalk("color", "black")').blackwx
            .six.columns
                  img#dctrlverse(src="./../assets/images/dctrlverse.png")
        .row
            .six.grid
                img#sundogepurp(src="./../assets/images/ao.svg")
            .six.grid
                shared-title(title='What is an Autonomous Organization?')
                p On site server, and raspberry pis serving open source web application
                p Attempt at fairness and reduced administration while sharing.
                p Holographic communication, organization, and prioritization tool.
                a(href='https://github.com/AutonomousOrganization') github.com/AutonomousOrganization
        p Decentralization fails without accepting responsibility and striving to create. Exploring dogeconciousness is worthy.
        img#sundogepurp(src="./../assets/images/sundogepurp.png")
</template>

<script>

import Hypercard from "./Card"
import BountyCard from "./Bounties/BountyCard"
import request from "superagent"
import SharedTitle from './slotUtils/SharedTitle'
import CrazyBtn from './slotUtils/CrazyBtn'
import calculations from './../calculations'
import TaskCreate from './forms/TaskCreate'
import WhyLightning from './Nodes/WhyLightning'

export default {
  data(){
      return {
          banner: 'Hello, world.'
      }
  },
  methods: {
      trySidewalk(show, color){
        request
            .put('/sidewalk')
            .send({
                type: 'sidewalk',
                show,
                color,
                banner: this.banner
            })
            .end((err,res) => {
                if (err) return console.log(err);
                console.log('createPayRec:', res.body)
            })
      },
  },
  components:{
      SharedTitle,
      Hypercard,
      CrazyBtn,
      BountyCard,
      TaskCreate,
      WhyLightning,
  },
}

</script>

<style lang="stylus" scoped>

@import './../styles/colours'
@import './../styles/skeleton'
@import './../styles/grid'
@import './../styles/button'

#wrex
    width: 100%
    #vine
        width: 100%
        height: auto

#broom
    z-index: 10000
    float: right
    height: 4.9em

#sundogepurp
    width:100%
    max-height:auto

#dctrlverse
    width:100%
    max-height:auto

button
    margin: .55em

p
    padding: 1.4em
    font-size: 1.4em


</style>
