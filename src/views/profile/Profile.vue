<template>
  <div id="profile">
    <nav-bar class="nav-bar"><div slot="center">小码哥商城</div></nav-bar>
    <!--1.单独封装一个组件: 利用slot知识点-->
    <UserInfo></UserInfo>

    <!--2.没有单独封装: 不同的地方太多, 需要传过多的参数-->
    <section class="account">
      <div class="account-item">
        <div class="number">
          <span class="balance">0.00</span>元
        </div>
        <div class="account-info">我的余额</div>
      </div>
      <div class="account-item">
        <div class="number">
          <span class="balance">0</span>个
        </div>
        <div class="account-info">我的优惠</div>
      </div>
      <div class="account-item">
        <div class="number">
          <span class="balance">0</span>分
        </div>
        <div class="account-info">我的积分</div>
      </div>
    </section>

    <!--3.封装成一个整体-->
    <list-view :list-data="orderList" class="order-list"></list-view>
    <list-view :list-data="serviceList" class="service-list"></list-view>
  </div>
</template>

<script>
  import UserInfo from './childComps/UserInfo'
  import ListView from './childComps/ListView'
  import NavBar from 'components/common/navbar/NavBar'

	export default {
		name: "Profile",
    components: {
		  UserInfo, ListView, NavBar
    },
    data: function () {
		  return {
		    orderList: [
          {icon: '#order', iconColor: '#ff8198', info: '我的消息'},
          {icon: '#point', iconColor: '#fc7b53', info: '积分商城'},
          {icon: '#vip', iconColor: '#ffc636', info: '会员卡'},
        ],
        serviceList: [
          {icon: '#service', iconColor: '#ff8198', info: '我的购物车'},
          {icon: '#download', iconColor: '#ff8198', info: '下载购物APP'},
        ]
      }
    },
    mounted: function () {
    }
	}
</script>

<style scoped>
  #profile {
    background-color: #f2f2f2;
  }

  .nav-bar {
    background-color: var(--color-tint);
    font-weight: 700;
    color: #fff;
  }

  .account {
    display: flex;
  }

  .account-item {
    width: 100%;
    background-color: #fff;
    margin-right: 1px;
    text-align: center;
  }

  .account-item:last-of-type {
    margin-right: 0;
  }

  .account-item {
    color: #666;
    font-size: 13px;
    padding: 18px;
  }

  .account-item .balance {
    font-size: 24px;
    font-weight: 700;
    color: #ff5f3e;
  }

  .account-info {
    margin-top: 6px;
  }

  .order-list, .service-list {
    margin-top: 12px;
  }

</style>




<!-- <template>
	<div>
		<scroll class="content">
			<ul>
				<li>个人信息1</li>
				<li>个人信息2</li>
				<li>个人信息3</li>
				<li>个人信息4</li>
				<li>个人信息5</li>
				<li>个人信息6</li>
				<li>个人信息7</li>
				<li>个人信息8</li>
				<li>个人信息9</li>
				<li>个人信息10</li>
				<li>个人信息11</li>
				<li>个人信息12</li>
				<li>个人信息13</li>
				<li>个人信息14</li>
				<li>个人信息15</li>
				<li>个人信息16</li>
				<li>个人信息17</li>
				<li>个人信息18</li>
				<li>个人信息19</li>
				<li>个人信息20</li>
				<li>个人信息21</li>
				<li>个人信息22</li>
				<li>个人信息23</li>
				<li>个人信息24</li>
				<li>个人信息25</li>
				<li>个人信息26</li>
				<li>个人信息27</li>
				<li>个人信息28</li>
				<li>个人信息29</li>
				<li>个人信息30</li>
				<li>个人信息31</li>
				<li>个人信息32</li>
				<li>个人信息33</li>
				<li>个人信息34</li>
				<li>个人信息35</li>
				<li>个人信息36</li>
				<li>个人信息37</li>
				<li>个人信息38</li>
				<li>个人信息39</li>
				<li>个人信息40</li>
				<li>个人信息41</li>
				<li>个人信息42</li>
				<li>个人信息43</li>
				<li>个人信息44</li>
				<li>个人信息45</li>
				<li>个人信息46</li>
				<li>个人信息47</li>
				<li>个人信息48</li>
				<li>个人信息49</li>
				<li>个人信息50</li>
				<li>个人信息51</li>
				<li>个人信息52</li>
				<li>个人信息53</li>
				<li>个人信息54</li>
				<li>个人信息55</li>
				<li>个人信息56</li>
				<li>个人信息57</li>
				<li>个人信息58</li>
				<li>个人信息59</li>
				<li>个人信息60</li>
				<li>个人信息61</li>
				<li>个人信息62</li>
				<li>个人信息63</li>
				<li>个人信息64</li>
				<li>个人信息65</li>
				<li>个人信息66</li>
				<li>个人信息67</li>
				<li>个人信息68</li>
				<li>个人信息69</li>
				<li>个人信息70</li>
				<li>个人信息71</li>
				<li>个人信息72</li>
				<li>个人信息73</li>
				<li>个人信息74</li>
				<li>个人信息75</li>
				<li>个人信息76</li>
				<li>个人信息77</li>
				<li>个人信息78</li>
				<li>个人信息79</li>
				<li>个人信息80</li>
				<li>个人信息81</li>
				<li>个人信息82</li>
				<li>个人信息83</li>
				<li>个人信息84</li>
				<li>个人信息85</li>
				<li>个人信息86</li>
				<li>个人信息87</li>
				<li>个人信息88</li>
				<li>个人信息89</li>
				<li>个人信息90</li>
				<li>个人信息91</li>
				<li>个人信息92</li>
				<li>个人信息93</li>
				<li>个人信息94</li>
				<li>个人信息95</li>
				<li>个人信息96</li>
				<li>个人信息97</li>
				<li>个人信息98</li>
				<li>个人信息99</li>
				<li>个人信息100</li>
			</ul>
		</scroll>
	</div>
</template>

<script>
	import Scroll from 'components/common/scroll/Scroll'
	
	export default{
		name:"Profile",
		components:{
			Scroll
		}
	}
</script>

<style scoped>
	.content{
		height: 300px;
		background-color: red;
		overflow: hidden;
	}
</style>
 -->