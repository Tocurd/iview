<template>
    <div style="margin: 200px;">
        <Select v-model="test" :transfer="true">
            <Option value="dd" lable='123'>12314</Option>
        </Select>

        <Select v-model="test1" :transfer="true">
            <Option value="dd" lable='123'>12314</Option>
        </Select>

        <Select v-model="test2" :transfer="true">
            <Option value="dd" lable='123'>12314</Option>
        </Select>

        <Select v-model="test3" :transfer="true">
            <Option value="dd" lable='123'>12314</Option>
        </Select>

        <Select v-model="test4" :transfer="true">
            <Option value="dd" lable='123'>12314</Option>
        </Select>

        <Select v-model="test5" :transfer="true">
            <Option value="dd" lable='123'>12314</Option>
        </Select>

        <Select v-model="test6" :transfer="true">
            <Option value="dd" lable='123'>12314</Option>
        </Select>


        <div style="height: 5500px;width: 200px;background-color: #000">
            
        </div>
        <!-- <Select size="small" v-model="model10" multiple style="width:260px">
            <Option v-for="item in cityList" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
        <Select v-model="model10" multiple style="width:260px">
            <Option v-for="item in cityList" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
        <Select size="large" v-model="model10" multiple style="width:260px">
            <Option v-for="item in cityList" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select> -->
    </div>
</template>
<script>
    export default {
        data () {
            return {
                test : "",
                test1 : "",
                test2 : "",
                test3 : "",
                test4 : "",
                test5 : "",
                test6 : "",
                cityList: [
                    {
                        value: 'New York',
                        label: 'New York'
                    },
                    {
                        value: 'London',
                        label: 'LondonLondonLondonLondonLondonLondonLondonLondonLondonLondonLondonLondonLondonLondonLondonLondonLondonLondon'
                    },
                    {
                        value: 'Sydney',
                        label: 'Sydney'
                    },
                    {
                        value: 'Ottawa',
                        label: 'Ottawa'
                    },
                    {
                        value: 'Paris',
                        label: 'Paris'
                    },
                    {
                        value: 'Canberra',
                        label: 'Canberra'
                    }
                ],
                model10: ['New York', 'London']
            }
        }
    }
</script>
