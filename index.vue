<template>
<div>
    <Row>
        <i-col :span='12'>
            <div>
                <i-button @click="modal1=true">新建目录</i-button>
            </div>
            <div>
                <Row>
                    <i-col :span='12'>
                        <h4>目录名称</h4>
                    </i-col>
                    <i-col :span='12'>
                        <h4>创建时间</h4>
                    </i-col>
                </Row>
                <div style="height:calc(100vh - 120px);overflow:auto">
                    <Row v-for="(item,index) in buckets" :key="index" style="padding:10px">
                        <i-col :span='12'>
                            <div @click="bucketName=item.name;listObjects()" class="file-name" :class="{checked:bucketName==item.name}">
                                <svg t="1618888228706" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2908" width="25" height="25">
                                    <path d="M848.8576 199.1936H415.7568c0-26.5728-21.5424-48.128-48.128-48.128H175.1424c-26.5728 0-48.128 21.5424-48.128 48.128V343.5648c0 26.5984 21.5424 48.1408 48.128 48.1408h673.728c26.5728 0 48.128-21.5424 48.128-48.1408v-96.2432c-0.0128-26.5856-21.5552-48.128-48.1408-48.128z" fill="#CCA352" p-id="2909"></path>
                                    <path d="M800.7424 247.3088H223.2576c-26.5728 0-48.128 21.5424-48.128 48.128v48.128c0 26.5984 21.5424 48.1408 48.128 48.1408h577.472c26.5728 0 48.128-21.5424 48.128-48.1408v-48.128c0-26.5728-21.5424-48.128-48.1152-48.128z" fill="#FFFFFF" p-id="2910"></path>
                                    <path d="M848.8576 295.4368H175.1424c-26.5728 0-48.128 21.5424-48.128 48.128v481.2544c0 26.5472 21.5424 48.128 48.128 48.128h673.728c26.5728 0 48.128-21.568 48.128-48.128V343.552c-0.0128-26.5728-21.5552-48.1152-48.1408-48.1152z" fill="#FFCC66" p-id="2911"></path>
                                </svg>
                                <span style="vertical-align: super;padding-left:5px">{{item.name}}</span>

                            </div>
                        </i-col>
                        <i-col :span="6">
                            <p>{{filterTime(item.creationDate)}}</p>
                        </i-col>
                        <i-col :span="6">
                            <Poptip confirm title="删除确认?" @on-ok="removeBucket(item.name)" placement="left-start">
                                <i-button type="error" size="small">删除</i-button>
                            </Poptip>

                        </i-col>
                    </Row>

                </div>
            </div>

        </i-col>
        <i-col :span='12'>
            <div>
                <input :disabled="!bucketName" ref="files" type="file" @change="putObject($event)">
            </div>
            <div>
                <Row>
                    <i-col :span='12'>
                        <h4>文件名称</h4>
                    </i-col>
                    <i-col :span='12'>
                        <h4>最后更新时间</h4>
                    </i-col>
                </Row>
                <Row v-for="(item,index) in objectData" :key="index" style="padding:10px">
                    <i-col :span='12'>
                        <div @click="getObject(item.name)" class="file-name">
                            <svg t="1618889665233" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3878" width="25" height="25">
                                <path d="M410.2 430.6h-4.8v91.6h4.8c24 0 41-11 41-46.4s-17-45.2-41-45.2zM282.6 430.4H268v43h15.6c17.4 0 25.8-8.2 25.8-22.8 0-15-9.6-20.2-26.8-20.2z" fill="#FF6969" p-id="3879"></path>
                                <path d="M791.94 155.36H332.48c-42.23 0-76.54 34.31-76.54 76.54v76.55h-51.03c-28.16 0-51.04 22.87-51.04 51.03v261.37c0 28.15 22.88 51.02 51.04 51.02h51.03v121.54c0 42.24 34.32 76.55 76.54 76.55h459.46c42.23 0 76.54-34.32 76.54-76.54V231.9c0.1-42.24-34.31-76.54-76.54-76.54zM268 501.6V551h-35.8V402.2h52.2c32.8 0 59.8 11.6 59.8 48.4 0 35.4-27.4 51-58.8 51H268z m549.45 291.9c-0.02 14.09-11.43 25.5-25.52 25.51H332.48c-14.09-0.01-25.5-11.43-25.51-25.51V671.96h306.21c28.15 0 51.03-22.88 51.03-51.03v-6.03h76.69c14.09-0.01 25.51-11.43 25.52-25.52-0.02-14.09-11.43-25.5-25.52-25.51h-76.69v-51.03h76.69c14.09-0.01 25.51-11.43 25.52-25.52-0.01-14.09-11.43-25.5-25.52-25.52h-76.69v-51.03h76.69c14.09-0.01 25.51-11.43 25.52-25.51-0.01-14.09-11.43-25.51-25.52-25.52h-76.69c0-28.16-22.89-51.03-51.03-51.03H306.97v-76.54c0.01-14.09 11.43-25.5 25.51-25.52h459.46c14.08 0.01 25.5 11.43 25.51 25.52V793.5zM369.6 551V402.2h42.8c45.4 0 75.4 21.8 75.4 73.6s-30 75.2-73.4 75.2h-44.8zM602 464.4v29.8h-50.8V551h-35.8V402.2h95v29.6h-59.2v32.6H602z" fill="#FF6969" p-id="3880"></path>
                            </svg>
                            <span style="vertical-align: super;padding-left:5px">{{item.name}}</span>

                        </div>
                    </i-col>
                    <i-col :span="6">
                        <p>{{filterTime(item.lastModified)}}</p>
                    </i-col>
                    <i-col :span="6">
                        <Poptip confirm title="删除确认?" @on-ok="removeObject(item.name)" placement="left-start">
                            <i-button type="error" size="small">删除</i-button>
                        </Poptip>
                    </i-col>
                </Row>
            </div>
        </i-col>
    </Row>
    <Modal v-model='modal1' title='新建目录' @on-ok='makeBucket'>
        <Input v-model="name"></Input>
    </Modal>
</div>
</template>

<script>
function ByteArray() {
    this.list = [];
    this.byteOffset = 0;
    this.length = 0;
}
var p = ByteArray.prototype;
p.push = function (unit8Arr) {
    this.list.push(unit8Arr);
    this.length += unit8Arr.length;
}
p.readBytes = function (len) {
    if (len > 0) {
        let rbuf = new Uint8Array(len);
        let rbuf_ind = 0;
        while (rbuf_ind < len) {
            if (this.list.length > 0) {
                let tmpbuf = this.list.shift();
                let tmplen = tmpbuf.length;
                let last_len = len - rbuf_ind;
                if (tmplen >= last_len) {
                    //足夠了
                    let tmpbuf2 = tmpbuf.subarray(0, last_len);
                    rbuf.set(tmpbuf2, rbuf_ind);
                    rbuf_ind += tmpbuf2.length;
                    if (last_len < tmplen) {
                        let newUint8Array = tmpbuf.subarray(last_len, tmplen);
                        this.list.unshift(newUint8Array);
                    }
                    break;
                } else {
                    rbuf.set(tmpbuf, rbuf_ind);
                    rbuf_ind += tmplen;
                }
            } else {
                rbuf = rbuf.subarray(0, rbuf_ind);
                break;
            }
        }
        this.length -= rbuf.length;
        return rbuf;
    }
    return null;
}

function arrayBufferToBase64(array) {
    var length = array.byteLength;
    var table = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H',
        'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P',
        'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X',
        'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f',
        'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n',
        'o', 'p', 'q', 'r', 's', 't', 'u', 'v',
        'w', 'x', 'y', 'z', '0', '1', '2', '3',
        '4', '5', '6', '7', '8', '9', '+', '/'
    ];
    var base64Str = '';
    for (var i = 0; length - i >= 3; i += 3) {
        var num1 = array[i];
        var num2 = array[i + 1];
        var num3 = array[i + 2];
        base64Str += table[num1 >>> 2] +
            table[((num1 & 0b11) << 4) | (num2 >>> 4)] +
            table[((num2 & 0b1111) << 2) | (num3 >>> 6)] +
            table[num3 & 0b111111];
    }
    var lastByte = length - i;
    if (lastByte === 1) {
        var lastNum1 = array[i];
        base64Str += table[lastNum1 >>> 2] + table[((lastNum1 & 0b11) << 4)] + '==';
    } else if (lastByte === 2) {
        var lastNum1 = array[i];
        var lastNum2 = array[i + 1];
        base64Str += table[lastNum1 >>> 2] +
            table[((lastNum1 & 0b11) << 4) | (lastNum2 >>> 4)] +
            table[(lastNum2 & 0b1111) << 2] +
            '=';
    }
    return base64Str;
}
    import {
        Client
    } from 'minio'
import * as fs from 'fs'
export default {
    data() {
        return {
            buckets: [],
            minioClient: null,
            modal1: false,
            name: '',
            bucketName: '',
            objectData: [],
            objectName: ''
        }
    },
    mounted() {

        this.minioClient = new Client({
            endPoint: window.IPS.minioIp,
            port: window.IPS.minioPort,
            useSSL: false,
            accessKey: window.IPS.accessKey,
            secretKey: window.IPS.secretKey,
            // accessKey: 'admin',
            // secretKey: 'root.2019',
        })
        console.log(this.minioClient)
        this.listBuckets()
    },
    methods: {
        listBuckets() {
            this.minioClient.listBuckets((err, buckets) => {
                if (err) console.log(err)
                else {
                    this.buckets = buckets
                    console.log('list')
                    console.log(this.buckets)
                }
            })
        },
        //判断目录是否存在
        bucketExists(bucketName){
            this.minioClient.bucketExists(bucketName,(err,exists)=>{
                if(err){
                    this.$Message.error(err)
                    return
                }
                if(!exists){
                    this.minioClient.makeBucket(
                        bucketName,
                        'cn-north-1',
                        (err) => {
                            if (!err) {
                                // this.$Message.success('新建成功！')
                                // this.listBuckets()
                                // this.modal1 = false
                            } else {
                                // this.$Message.error(err)
                            }
                        }
                    )
                }
            })
        },
        makeBucket() {
            if (!this.name) return
            this.minioClient.makeBucket(
                this.name,
                'cn-north-1',
                (err) => {
                    if (!err) {
                        this.$Message.success('新建成功！')
                        this.listBuckets()
                        this.modal1 = false
                    } else {
                        this.$Message.error(err)
                    }
                }
            )
        },
        removeBucket(bucketName) {
            this.minioClient.removeBucket(bucketName, (err) => {
                if (err) {
                    this.$Message.error('改目录下有文件')
                    return
                }
                this.listBuckets()
                this.objectData = []
            })
        },
        filterTime(creationDate) {
            return new Date(creationDate).toLocaleString()
        },
        //获取目录中的文件
        listObjects() {
            this.objectData = []
            let stream = this.minioClient.listObjectsV2(
                this.bucketName, '', true
            )
            stream.on('data', (obj) => {
                this.objectData.push(obj)
            })
        },
        //删除文件
        removeObject(objectName) {
            this.minioClient.removeObject(this.bucketName, objectName,
                (err) => {
                    if (err) {
                        this.$Message.error(err)
                        return
                    }
                    this.listObjects()
                })
        },
        //下载文件
        getObject(objectName) {
            let size = 0,
                data = new ByteArray()
            this.minioClient.getObject(this.bucketName, objectName,
                (err, dataStream) => {
                    if (err) {
                        return console.log(err)
                    }
                    dataStream.on('data', function (chunk) {
                        data.push(chunk)
                        size += chunk.length
                    })
                    dataStream.on('end', () => {
                        console.log('End. Total size = ' + size)
                        // console.log(arrayBufferToBase64(data.readBytes(size)))
                        // console.log(new Buffer(data))
                        // console.log(new Blob([data],{type:}))
                        // return
                        // let newData = new Uint8Array(data)
                            // a.href = 'data:application/pdf;base64,'+arrayBufferToBase64(data.readBytes(size));
                        var reader = new FileReader();
                        reader.readAsDataURL(new Blob([data.readBytes(size)],{
                            type:'application/pdf'
                        }));
                        // reader.readAsArrayBuffer(data.readBytes(size));
                        reader.onload = e => {
                            var a = document.createElement('a');
                            // 获取文件名fileName
                            a.download = objectName;
                            a.href = e.target.result;
                            document.body.appendChild(a);
                            a.click();
                            document.body.removeChild(a);
                        }

                    })
                    dataStream.on('error', function (err) {
                        console.log(err)
                    })
                })
        },
        // 上传文件
        putObject(e) {
            let file = e.target.files[0]
            // console.log(this.$refs)
            // console.log(window.URL.createObjectURL(file))
            // return
            var reader = new FileReader();
            reader.readAsDataURL(file);
            reader.onload = (ev) => {
                var arr = ev.target.result.split(','),
                    mime = arr[1]
                let fileData = Buffer.from(mime, 'base64')
                this.minioClient.putObject(this.bucketName, file.name, fileData, file.size, (err, etag) => {
                    if (err) {
                        console.log(err)
                    } else {
                        this.listObjects()
                    }
                })
            }
        }
    }
}
</script>

<style scoped>
.file-name {
    cursor: pointer;
    display: inline-block;
    padding: 0 10px;
    border-radius: 8px;
}

.file-name:hover {
    color: #fff;
}

.file-name.checked {
    background: #0e8a97;
    color: #fff;
}
</style>
