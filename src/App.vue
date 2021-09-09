<template>
  <div id="app" class="flex container h-screen w-full">

    <div id="sidenav" class="lg:w-1/5 border-r border-lighter px-2 lg:px-6 py-2 flex flex-col justify-between">
      <div >
        <button class="h-12 w-12 hover:bg-lightblue text-3xl rounded-full text-blue">
        <i class="fab fa-twitter"></i>
      </button>
      <div>
        <button :key="tab" @click="id=tab.id" :class="`focus:outline-none hover:text-blue flex items-center py-2 px-4 hover:bg-lightblue rounded-full mr-auto mb-3 ${ id === tab.id ? 'text-blue' : ''}`"
                v-for="tab in tabs">
          <i :class="`${ tab.icon } text-2xl mr-4 text-left`"></i>
          <p class="text-lg font-semibold text-left hidden lg:block"> {{ tab.title }} </p>
        </button>
      </div>
      <button class="text-white bg-blue rounded-full font-semibold focus:outline-none lg:h-auto lg:w-full p-3 hover:bg-darkblue">
        <p class="hidden lg:block">Tweet</p>
        <i class="fas fa-plus lg:hidden"></i>
      </button>
      </div>
      <div class="lg:w-full relative">
        <button @click="appear=true" class="flex items-center w-full hover:bg-lightbue rounded-full focus:outline-none">
          <img src="pp.jpg" alt="" class="w-12 h-12 rounded-full border border-lighter">
          <div class="hidden lg:block ml-4">
            <p class="text-sm font-bold leading-tight">Baed Savitar</p>
            <p class="text-sm leading-tight">@thinker732</p>
          </div>
          <i class="hidden lg:block fas fa-angle-down ml-auto text-lg"></i>
        </button>

        <div v-if="appear===true" class="absolute bottom-0 left-0 w-64 round-lg shadow-xl border-lightest bg-white mb-16">
          <button @click="appear=false" class="p-3 flex items-center w-full hover:bg-lightest  focus:outline-none">
            <img src="watch.jpg" alt="" class="w-12 h-12 rounded-full border border-lighter">
            <div class="ml-4">
              <p class="text-sm font-bold leading-tight">Baed Savitar</p>
              <p class="text-sm leading-tight">@thinker732</p>
            </div>
            <i class="fas fa-check ml-auto text-black"></i>
          </button>
          <button @click="appear=false"  class="w-full focus:outline-none text-left hover:bg-lightest border-t border-lighter p-3 text-sm">
            Add an  Account
          </button>
          <button @click="appear=false"  class="w-full focus:outline-none text-left hover:bg-lightest border-t border-lighter p-3 text-sm">
            Log Out
          </button>
        </div>
      </div>
    </div>


    <div id="tweets" class="w-1/2 h-full overflow-y-scroll">
      <div class="px-5 py-2 border-b border-lighter flex items-center justify-between">
        <h1 class="text-xl font-bold">Home</h1>
        <i class="far fa-star text-xl text-blue"></i>
      </div>
      <div class="px-5 py-3 border-b-8 border-lighter flex">
        <div class="flex-none">
          <img src="watch.jpg" class="flex-none w-12 h-12 rounded-full border border-lighter"/>
        </div>
        <form  v-on:submit.prevent="addNewTweet" class="w-full px-4 relative">
          <textarea  v-model="tweet.content" placeholder="What's up?" class="mt-3 pb-3 w-full focus:outline-none pl-5"/>
          <div class="flex items-center">
            <i class="text-lg text-blue mr-4 far fa-image"></i>
            <i class="text-lg text-blue mr-4 fas fa-film"></i>
            <i class="text-lg text-blue mr-4 fas fa-poll-h"></i>
            <i class="text-lg text-blue mr-4 far fa-smile"></i>
            <i class="text-lg text-blue mr-4 far fa-calendar-alt"></i>
          </div>
          <button type="submit" class="h-10 px-4 text-white font-semibold bg-blue hover:bg-darblue focus:outline-none rounded-full absolute  absolute bottom-0 right-0">
            Tweet
          </button>
        </form>
      </div>
      <div class="flex flex-col-reverse">
        <div v-for="tweet in tweets" :key="tweet" class="w-full p-4 border-b hover:bg-lighter flex">
          <div class="flex-none mr-4">
            <img src="watch.jpg" class="h-12 w-12 rounded-full flex-none"/>
          </div>
          <div class="w-full">
            <div class="flex items-center w-full">
              <p class="font-semibold"> Baed Savitar </p>
              <p class="text-sm text-dark ml-2"> @thinker732 </p>
              <p class="text-sm text-dark ml-2"> {{tweet.time}}</p>
              <i class="fas fa-angle-down text-dark ml-auto"></i>
            </div>
            <p class="py-2">
              {{ tweet.content }}
            </p>
            <div class="flex items-center justify-between w-full">
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-comment mr-3 hover:text-blue-400"></i>
                <p> 0 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-retweet mr-3 hover:text-green-400"></i>
                <p> 0 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i @click="like=true" :class="`fas fa-heart mr-3 hover:text-red-400 ${like === true?'text-red-600' : ''}`"></i>
                <p> 0 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-share-square mr-3"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-for="follow in following" :key="follow" class="w-full p-4 border-b hover:bg-lighter flex">
        <div class="flex-none mr-4">
          <img :src="`${follow.src}`" class="h-12 w-12 rounded-full flex-none"/>
        </div>
        <div class="w-full">
          <div class="flex items-center w-full">
            <p class="font-semibold"> {{ follow.name }} </p>
            <p class="text-sm text-dark ml-2"> {{ follow.handle }} </p>
            <p class="text-sm text-dark ml-2"> {{ follow.time }} </p>
            <i class="fas fa-angle-down text-dark ml-auto"></i>
          </div>
          <p class="py-2">
            {{ follow.tweet }}
          </p>
          <div class="flex items-center justify-between w-full">
            <div class="flex items-center text-sm text-dark">
              <i class="far fa-comment mr-3 hover:text-blue-600"></i>
              <p> {{ follow.comments }} </p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-retweet mr-3 hover:text-green-400"></i>
              <p> {{ follow.retweets }} </p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-heart mr-3 hover:text-red-400"></i>
              <p> {{ follow.like }} </p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-share-square mr-3"></i>
            </div>
          </div>
        </div>
      </div>

      </div>


    <div id="trending" class="md:block hidden w-1/3 h-full border-l border-lighter px-2 px-6 pt-8 overflow-y-scroll relative ">
      <input class="pl-12 rounded-full w-full p-2 text-sm mb-4  bg-lighter focus:outline-none" placeholder="search"/>
      <i class="fa fa-search absolute left-0 top-0 mt-12 ml-12 text-sm text-light">
      </i>

      <div class="w-full rounded-lg bg-lightest" id="tendances">
        <div class="flex items-center justify-between p-3">
          <p class="text-lg font-bold">Trends for You</p>
          <i class="fas fa-cog text-lg text-blue"></i>
        </div>
        <button v-for="trend in trending" :key="trend" class="w-full flex justify-between hover:bg-lighter p-3 border-t border-lighter">
          <div>
          <p class="text-xs text-left leading-tight text-dark">{{trend.top}}</p>
          <p class="font-bold text-left leading-tight text-dark">{{trend.title}}</p>
          <p class="font-bold text-left leading-tight text-dark">{{trend.bottom}}</p>
          </div>
          <i class="fas fa-angle-down text-lg text-dark"></i>
        </button>
        <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter">
          Show More
        </button>
      </div>


      <div class="w-full rounded-lg bg-lightest mt-6 my-4" id="suggestions">
        <div class="p-3">
          <p class="text-lg font-bold">Suggestions</p>
        </div>
        <button v-for="friend in friends" :key="friend" class="w-full flex  hover:bg-lighter p-3 border-t border-lighter ">
          <img :src="`${friend.src}`" alt="" class="w-12 h-12 rounded-full border border-lighter">
          <div class="ml-4">
            <p class="text-sm font-bold leading-tight">{{friend.name}}</p>
            <p class="text-sm leading-tight">{{friend.handle}}</p>
          </div>
          <button class="ml-auto text-sm text-blue py-1 px-4 rounded-full border-2 border-blue focus:outline-none ">
            Follow
          </button>
        </button>
        <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter">
          Show More
        </button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        tabs: [
          {icon: 'fas fa-home', title: 'Home', id: 'home'},
          {icon: 'fas fa-hashtag', title: 'Explore', id: 'explore'},
          {icon: 'fas fa-bell', title: 'Notifications', id: 'notifications'},
          {icon: 'fas fa-envelope', title: 'Messages', id: 'messages'},
          {icon: 'fas fa-bookmark', title: 'Bookmarks', id: 'bookmarks'},
          {icon: 'fas fa-clipboard-list', title: 'Lists', id: 'lists'},
          {icon: 'fas fa-user', title: 'Profile', id: 'profile'},
          {icon: 'fas fa-ellipsis-h', title: 'More', id: 'more'},
        ],
        id: 'home',
        appear: false,
        like: false,
        trending: [
          {top: 'Trending in Cameroon', title: '#corona', bottom: '50k tweets'},
          {top: 'Music', title: 'k-pop', bottom: '21K Tweets'},
          {top: 'Coding', title: 'Js', bottom: '40k tweets'},
          {top: 'Trending in Cameroon', title: 'Amen', bottom: '144k tweets'},
        ],
        friends: [
          {src: 'vue.png', name: 'Vue.js', handle: '@vuejs'},
          {src: 'tailwind.jpg', name: 'tailwindcss', handle: '@tailwindcss'},
          {src: 'js.png', name: 'JS', handle: '@javascript'}
        ],
        following: [
          {
            src: 'the_one.jpg',
            name: 'Escanor',
            handle: '@the_one',
            time: '10 min',
            tweet: 'who can beat me?',
            comments: '7,000',
            retweets: '163',
            like: '1,5k'
          },
          {
            src: 'shadow_master.jpg',
            name: 'Shadow Master',
            handle: '@me732',
            time: '45 min',
            tweet: 'Should i build the dark mode',
            comments: '121',
            retweets: '02',
            like: '101'
          },
          {
            src: 'dark.jpg',
            name: 'the_hacker',
            handle: '@IOI',
            time: '1.1 hr',
            tweet: 'should i hack nasa?',
            comments: '105k',
            retweets: '32',
            like: '103'
          },
          {
            src: 'the_one.jpg',
            name: 'Escanor',
            handle: '@the_one',
            time: '1 hr',
            tweet: 'lion of proud ,Escanor',
            comments: '10k',
            retweets: '700k',
            like: '100,000'
          },
          {
            src: 'b.jpg',
            name: 'les_b-man',
            handle: '@jinchuruki',
            time: '1.1 hr',
            tweet: 'yeeeeeeeeeeeeeeeeeaaaaaaaaaaaaaaaaaahhhhhhhhhhhh',
            comments: '5,000',
            retweets: '32',
            like: '103'
          }
        ],
        tweets: [
          {content: 'Hello twitter',time:'2min'}
        ],
        tweet: {content: ''}
      }
    },
    methods: {
      addNewTweet() {
        let newTweet = {
          content: this.tweet.content,
          time:'1sec'
        };
        this.tweets.push(newTweet)
      }
    }
  }
</script>
