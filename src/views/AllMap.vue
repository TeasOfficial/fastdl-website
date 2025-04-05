<script>
import { ref } from 'vue'
import axios from 'axios';
import { downloadFile } from 'vue-amazing-ui';
export default {
    data() {
        return {
            notification: window['$notification'],
            spinning: ref(false),
            maplist: [],
            mapNum: 0,
            page: 1,
            value: ref(''),
            columns: [
                {
                    title: '地图名',
                    dataIndex: 'name',
                    key: 'name'
                },
                {
                    title: '操作',
                    width: 10,
                    key: 'action'
                }
            ]
        }
    },
    mounted() {
        this.spinning = true
        axios.get(document.location.protocol + "//d.ipairsdo.xin/maps_index.html.txt").then(
            (res) => {
                this.spinning = false
                res.data.split("\n").forEach(v => {
                    this.maplist.push({
                        name: v
                    })
                });
                this.mapNum = this.maplist.length
            }
        ).catch(
            (err) => {
                this.spinning = false
                this.maplist = []
            }
        )
    },
    methods: {
        onChange(page) {
        },
        onSearch(value) {
            this.spinning = true
            this.maplist = []
            this.mapNum = 0
            axios.get(document.location.protocol + "//d.ipairsdo.xin/maps_index.html.txt").then(
                (res) => {
                    this.spinning = false
                    res.data.split("\n").forEach(v => {
                        if (v.indexOf(value) != -1) {
                            this.maplist.push({
                                name: v
                            })
                        }
                    });
                    this.mapNum = this.maplist.length
                }
            ).catch(
                (err) => {
                    this.spinning = false
                    this.maplist = []
                }
            )
        },
        download(mapname) {
            var start = this.notification.open({
                title: '下载开始',
                description: "正在准备下载 " + mapname + " 请稍后...",
                duration: 1500
            })
            const iframe = document.createElement('iframe');
            iframe.style.display = 'none';
            iframe.src = 'https://main.fastdl.me/maps/' + mapname + '.bsp.bz2';
            iframe.onload = () => {
                window['$notification'].success({
                title: '下载就绪！',
                description: "您的下载已经开始！"
            })
            }
            document.body.appendChild(iframe);
            
        }
    }
}
</script>

<template>
    <Alert message="警告!!!" description="因数据较多，本页面可能会导致性能问题！" type="warning" show-icon closable close-text="我已知晓" />
    <Divider :border-width="3" border-color="orange">
        <!-- <GradientText type="warning" :size="16" :weight="700">
            已装载
             张地图
        </GradientText> -->
        <Statistic title="已装载地图数">
            <NumberAnimation :from="0" :to="mapNum" separator="" :duration="1500" />
        </Statistic>
    </Divider>
    <Spin :spinning="spinning" size="large" indicator="magic-ring">
        <InputSearch v-model:value="value" placeholder="在此处输入地图名" :icon="false" search="搜索"
            :search-props="{ type: 'primary' }" @search="onSearch" />
        <br><br>
        <Table :columns="columns" :dataSource="maplist" :show-size-changer="false" show-quick-jumper>
            <template #bodyCell="{ column, record }">
                <template v-if="column.key === 'action'">
                    <Button type="primary" shape="round" @click="download(record.name)">
                        下载
                    </Button>
                </template>
            </template>
        </Table>

        <InputSearch v-model:value="value" placeholder="在此处输入地图名" :icon="false" search="搜索"
            :search-props="{ type: 'primary' }" @search="onSearch" />
    </Spin>
</template>