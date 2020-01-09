<template>
    <div>
        <el-dialog
                title="提示"
                :visible.sync="dialogVisible"
                width="30%"
                :before-close="handleClose">
            <span>这是一段信息</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="dialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
            </span>
        </el-dialog>

        <h1>Album Manager</h1>

        <el-form :inline="true" :model="album" class="demo-form-inline">
            <el-form-item label="专辑ID">
                <el-input v-model="album.album_id" placeholder="请输入专辑ID"></el-input>
            </el-form-item>
            <el-form-item label="专辑名称">
                <el-input v-model="album.album_name" placeholder="请输入专辑名称"></el-input>
            </el-form-item>
            <el-form-item label="发布时间">
                <el-input v-model="album.public_time" placeholder="请输入发布时间"></el-input>
            </el-form-item>
            <el-form-item label="专辑week">
                <el-input v-model.number="album.week" type="number" placeholder="请输入week"></el-input>
            </el-form-item>
            <el-form-item label="专辑价格">
                <el-input v-model.number="album.price" type="number" placeholder="请输入专辑价格"></el-input>
            </el-form-item>
            <el-form-item label="封面地址">
                <el-input v-model="album.cover" placeholder="请输入封面地址"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="addAlbum">添加专辑</el-button>
            </el-form-item>
        </el-form>

        <el-table
                :data="albums"
                style="width: 100%">
            <el-table-column
                    prop="album_id"
                    label="专辑ID"
                    width="250">
            </el-table-column>

            <el-table-column
                    prop="album_name"
                    label="专辑名"
                    width="250">
            </el-table-column>
            <el-table-column
                    prop="price"
                    label="价格"
                    width="250">
            </el-table-column>

            <el-table-column
                    prop="singers"
                    label="歌手名"
                    width="250">
            </el-table-column>
            <el-table-column
                    fixed="right"
                    label="操作">
                <template slot-scope="albums">
                    <el-button  type="text" size="small">详细内容</el-button>
                    <el-button @click="dialogVisible=true" type="text" size="small">编辑</el-button>
                    <el-button type="text" size="small">追加歌手</el-button>
                    <el-button @click="deleteAlbum(albums.row)" type="text" size="small">删除</el-button>
                </template>
            </el-table-column>
        </el-table>
     </div>
</template>

<script>
    export default {
        name: "AlbumManager",
        created() {
            fetch(this.url, {"type": "GET"})
                .then(res => res.json())
                .then(bks => this.albums = bks)
        },
        data() {
            return {
                url: "http://localhost:3000/albums",
                album: {album_id: '', album_name: '',public_time: '',week: '',price: '',cover:''},
                albums: [],
                dialogVisible: false
            }
        },
        methods: {
            addAlbum() {
                fetch(this.url, {
                    method: "POST",
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(this.album)
                }).then(res => res.json())
                    .then(bk => this.albums.push(bk))
            },
            deleteAlbum(album) {
                fetch(this.url + "/" + album._id, {method: "DELETE"})
                    .then(res => res.json())
                    .then(() => {
                            let index = this.albums.findIndex(bk => bk._id == album._id)
                            this.albums.splice(index, 1)
                        }
                    )
            }
        },
     }
</script>

<style scoped>

</style>