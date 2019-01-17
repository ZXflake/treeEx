<template>
  <Tree :data="data5" :render="renderContent"></Tree>
</template>
<script>
    export default {
        data () {
            return {
                data5: [
                    {
                        title: 'parent 1',
                        expand: true,
                        render: (h, { root, node, data }) => {
                            return h('span', {
                                style: {
                                    display: 'inline-block',
                                    width: '100%'
                                }
                            }, [
                                h('span', [
                                    h('Icon', {
                                        props: {
                                            type: 'ios-folder-outline'
                                        },
                                        style: {
                                            marginRight: '8px'
                                        }
                                    }),
                                    h('span', data.title)
                                ]),
                                h('span', {
                                    style: {
                                        display: 'inline-block',
                                        float: 'right',
                                        marginRight: '32px'
                                    }
                                }, [
                                    h('Button', {
                                        props: Object.assign({}, this.buttonProps, {
                                            icon: 'ios-add',
                                            type: 'primary'
                                        }),
                                        style: {
                                            width: '64px'
                                        },
                                        on: {
                                            click: () => { this.append(root, node, data) }
                                        }
                                    })
                                ])
                            ]);
                        },
                        children: []
                    }
                ],
                buttonProps: {
                    type: 'default',
                    size: 'small',
                }
            }
        },
        methods: {
            renderContent (h, { root, node, data }) {
                let types = ["ios-analytics","ios-aperture","ios-appstore","md-aperture"]
                let L = this.level (root,node)
                let flag = node.node.edit === true
                return h('span', {
                    style: {
                        display: 'inline-block',
                        width: '100%'
                    }
                }, [
                    h('span',{
                        'class': 'level'+L
                    }, [
                        h('Icon', {
                            props: {
                                type: types[L || 0]
                            },
                            style: {
                                marginRight: '8px'
                            }
                        }),
                        //编辑状态使用input
                        flag ?
                            h('input', {
                                domProps: {
                                    value: data.title,
                                    title: data.title
                                },
                                'class':'ivu-input ivu-input-small',
                                style:{
                                    width:'150px'
                                },
                                on: {
                                    // 更新数据
                                    input: event=>{
                                        data.title = event.target.value
                                        //_this.$emit('input', event.target.value)
                                    }
                                },
                            })
                            :
                            h('span', data.title)
                    ]),
                    h('span', {
                        style: {
                            display: 'inline-block',
                            float: 'right',
                            marginRight: '32px'
                        }
                    }, [
                        h('Button', {
                            props: Object.assign({}, this.buttonProps, {
                                icon: 'ios-add'
                            }),
                            style: {
                                marginRight: '8px'
                            },
                            on: {
                                click: () => { this.append(root, node, data) }
                            }
                        }),
                        h('Button', {
                            props: Object.assign({}, this.buttonProps, {
                                icon: 'ios-remove'
                            }),
                            style: {
                                marginRight: '8px'
                            },
                            on: {
                                click: () => { this.remove(root, node, data) }
                            }
                        }),
                        flag ?
                            h('Button', {
                                props: Object.assign({}, this.buttonProps, {
                                    icon: 'md-close'
                                }),
                                on: {
                                    click: () => { this.editReset(root) }
                                }
                            })
                            :
                            h('Button', {
                                props: Object.assign({}, this.buttonProps, {
                                    icon: 'md-create'
                                }),
                                on: {
                                    click: () => { this.edits(root, node, data) }
                                }
                            })
                    ])
                ]);
            },
            append (root, node, data) {
                if(node && this.level (root,node) > 2){
                    alert("达到最大节点")
                    node.node.expand = false;
                    return ;
                }
                const children = data.children || [];
                children.push({
                    title: 'level'+(this.level (root,node) || 0),
                    expand: true,
                    edit:false,

                });
                this.$set(data || {}, 'children', children);
            },
            remove (root, node, data) {
                const parentKey = root.find(el => el === node).parent;
                const parent = root.find(el => el.nodeKey === parentKey).node;
                const index = parent.children.indexOf(data);
                parent.children.splice(index, 1);
            },
            edits (root, node, data) {
                // let result = prompt("("+data.title+")修改为")
                // if(result !== null) data.title = result
                this.editReset(root)
                //node.edit = true
                this.$set(node.node, 'edit', true);         //当前节点变为可修改

                //触发render函数
                // const children = data.children || [];
                // this.$set(data || {}, 'children', children);
                //console.log(this.level(root,node))

                //data.title = '<input value="'+data.title+'"/>'
            },
            /**
             * 关闭修改
             * @param root 根元素
             * @returns undefined
             */
            editReset (root){
                root.forEach(el => el.node.edit = false)    //全部关闭修改
            },
            /**
             * 获取当前节点级别（从0开始）
             * @param root 根元素
             * @param node 当前节点
             * @returns {number} 级别
             */
            level (root,node){
                let l = 0;
                let parentKey
                for(;;){
                    if(node.parent === undefined) return l
                    l++
                    parentKey = root.find(el => el === node).parent;
                    node = root.find(el => el.nodeKey === parentKey);
                }
            }
        }
    }
</script>
<style>
  .level1{
    color: #E8559E;
  }
  .level2{
    color: #F29F05;
  }
  .level3{
    color: #008C72;
  }

</style>