<template lang='pug'>

.priorityaction.clearboth
    router-link(:to='"/task/" + taskId')
        img.singleship(src='../../assets/images/singleship.svg')
    div.agedwrapper(:class="cardInputSty")
        .agedbackground.freshpaper
        span {{ name }} !!
        .row
            .six.grid
                button.accept(@click='claim')
                    img.arrow.fr(src='../../assets/images/buddadoge.svg')
                    span complete
            .six.grid
                button.dontaccept(@click='refuse')
                    img.arrow.fl(src='../../assets/images/buddadoge.svg')
                    span refocus

</template>

<script>

import request from 'superagent'

export default {
    data(){
        return {
            notes: ''
        }
    },
    props: ['taskId', 'nextAction', 'inId'],
    computed: {
        name(){
            let name
            this.$store.state.tasks.some(t => {
                if (this.taskId === t.taskId){
                    name = t.name
                    return true
                }
            })
            return name
        },
        cardInputSty() {
          let color
          this.$store.state.tasks.some(t => {
              if (this.taskId === t.taskId){
                  color = t.color
                  return true
              }
          })
          return {
              redwx : color == 'red',
              bluewx : color == 'blue',
              greenwx : color == 'green',
              yellowwx : color == 'yellow',
              purplewx : color == 'purple',
              blackwx : color == 'black',
          }
        }
    },
    methods: {
        claim(){
            request
                .post('/events')
                .set('Authorization', this.$store.state.loader.token)
                .send({
                    type: 'task-claimed',
                    taskId: this.taskId,
                    memberId: this.$store.getters.member.memberId,
                    inId: this.inId,
                    notes: this.notes,
                })
                .end((err,res)=>{

                })
        },
        refuse(){
            request
                .post('/events')
                .set('Authorization', this.$store.state.loader.token)
                .send({
                    type: 'task-refocused',
                    inId: this.inId, //does not exist
                    taskId: this.taskId,
                })
                .end((err,res)=>{

                })
        }
    }
}

</script>

<style lang='stylus' scoped>

@import '../../styles/colours'
@import '../../styles/grid'

button
    width: 100%
    span
        color : white
        font-weight: bolder;

.priorityaction
    color: white

.clearboth
    clear: both
    
.singleship
    width: 3.3724em
    margin-top: 0.5em

.arrow
    height: 3.35em
    border-radius: 3px

button
    img
        background: white
        padding: .1em


.accept, .dontaccept
    background: accent5
    padding: .789em
    border-style: none
    // img
.fr
  float:right

.fl
  float:left

.agedwrapper
    position: relative
    margin-top: 0.5em
    float: right
    padding: 0.5em
    width: calc(100% - 5.5em)
    margin-right: 0.5em

.agedbackground
    background-image: url('../../assets/images/paper.jpg')
    background-repeat: no-repeat
    background-position: center center
    background-size: cover
    top: 0
    left: 0
    bottom: 0
    right: 0
    position: absolute
    width: 100%
    height: 100%
    pointer-events: none
    //border-radius: 12px

.freshpaper
    background-image: url('../../assets/images/paper.jpg')
    opacity: 0.2

</style>
