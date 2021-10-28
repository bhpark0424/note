# Linux Kernel API

## Socket Buffer
### struct sk_buff 구조체 (linux 2.6.38)
```c
/**
 *      struct sk_buff - socket buffer
 *      @next: Next buffer in list
 *      @prev: Previous buffer in list
 *      @sk: Socket we are owned by
 *      @tstamp: Time we arrived
 *      @dev: Device we arrived on/are leaving by
 *      @transport_header: Transport layer header
 *      @network_header: Network layer header
 *      @mac_header: Link layer header
 *      @_skb_refdst: destination entry (with norefcount bit)
 *      @sp: the security path, used for xfrm
 *      @cb: Control buffer. Free for use by every layer. Put private vars here
 *      @len: Length of actual data
 *      @data_len: Data length
 *      @mac_len: Length of link layer header
 *      @hdr_len: writable header length of cloned skb
 *      @csum: Checksum (must include start/offset pair)
 *      @csum_start: Offset from skb->head where checksumming should start
 *      @csum_offset: Offset from csum_start where checksum should be stored
 *      @local_df: allow local fragmentation
 *      @cloned: Head may be cloned (check refcnt to be sure)
 *      @nohdr: Payload reference only, must not modify header
 *      @pkt_type: Packet class
 *      @fclone: skbuff clone status
 *      @ip_summed: Driver fed us an IP checksum
 *      @priority: Packet queueing priority
 *      @users: User count - see {datagram,tcp}.c
 *      @protocol: Packet protocol from driver
 *      @truesize: Buffer size
 *      @head: Head of buffer
 *      @data: Data head pointer
 *      @tail: Tail pointer
 *      @end: End pointer
 *      @destructor: Destruct function
 *      @mark: Generic packet mark
 *      @nfct: Associated connection, if any
 *      @ipvs_property: skbuff is owned by ipvs
 *      @peeked: this packet has been seen already, so stats have been
 *              done for it, don't do them again
 *      @nf_trace: netfilter packet trace flag
 *      @nfctinfo: Relationship of this skb to the connection
 *      @nfct_reasm: netfilter conntrack re-assembly pointer
 *      @nf_bridge: Saved data about a bridged frame - see br_netfilter.c
 *      @skb_iif: ifindex of device we arrived on
 *      @rxhash: the packet hash computed on receive
 *      @queue_mapping: Queue mapping for multiqueue devices
 *      @tc_index: Traffic control index
 *      @tc_verd: traffic control verdict
 *      @ndisc_nodetype: router type (from link layer)
 *      @dma_cookie: a cookie to one of several possible DMA operations
 *              done by skb DMA functions
 *      @secmark: security marking
 *      @vlan_tci: vlan tag control information
 */

struct sk_buff {
        /* These two members must be first. */
        struct sk_buff          *next;
        struct sk_buff          *prev;

        ktime_t                 tstamp;

        struct sock             *sk;
        struct net_device       *dev;

        /*
         * This is the control buffer. It is free to use for every
         * layer. Please put your private variables there. If you
         * want to keep them across layers you have to do a skb_clone()
         * first. This is owned by whoever has the skb queued ATM.
         */
        char                    cb[48] __aligned(8);

        unsigned long           _skb_refdst;
#ifdef CONFIG_XFRM
        struct  sec_path        *sp;
#endif
        unsigned int            len,
                                data_len;
        __u16                   mac_len,
                                hdr_len;
        union {
                __wsum          csum;
                struct {
                        __u16   csum_start;
                        __u16   csum_offset;
                };
        };
        __u32                   priority;
        kmemcheck_bitfield_begin(flags1);
        __u8                    local_df:1,
                                cloned:1,
                                ip_summed:2,
                                nohdr:1,
                                nfctinfo:3;
        __u8                    pkt_type:3,
                                fclone:2,
                                ipvs_property:1,
                                peeked:1,
                                nf_trace:1;
        kmemcheck_bitfield_end(flags1);
        __be16                  protocol;

        void                    (*destructor)(struct sk_buff *skb);
#if defined(CONFIG_NF_CONNTRACK) || defined(CONFIG_NF_CONNTRACK_MODULE)
        struct nf_conntrack     *nfct;
#endif
#ifdef NET_SKBUFF_NF_DEFRAG_NEEDED
        struct sk_buff          *nfct_reasm;
#endif
#ifdef CONFIG_BRIDGE_NETFILTER
        struct nf_bridge_info   *nf_bridge;
#endif

        int                     skb_iif;
#ifdef CONFIG_NET_SCHED
        __u16                   tc_index;       /* traffic control index */
#ifdef CONFIG_NET_CLS_ACT
        __u16                   tc_verd;        /* traffic control verdict */
#endif
#endif

        __u32                   rxhash;

        kmemcheck_bitfield_begin(flags2);
        __u16                   queue_mapping:16;
#ifdef CONFIG_IPV6_NDISC_NODETYPE
        __u8                    ndisc_nodetype:2,
                                deliver_no_wcard:1;
#else
        __u8                    deliver_no_wcard:1;
#endif
        __u8                    ooo_okay:1;
        kmemcheck_bitfield_end(flags2);

        /* 0/13 bit hole */

#ifdef CONFIG_NET_DMA
        dma_cookie_t            dma_cookie;
#endif
#ifdef CONFIG_NETWORK_SECMARK
        __u32                   secmark;
#endif
        union {
                __u32           mark;
                __u32           dropcount;
        };

        __u16                   vlan_tci;

        sk_buff_data_t          transport_header;
        sk_buff_data_t          network_header;
        sk_buff_data_t          mac_header;
        /* These elements must be at the end, see alloc_skb() for details.  */
        sk_buff_data_t          tail;
        sk_buff_data_t          end;
        unsigned char           *head,
                                *data;
        unsigned int            truesize;
        atomic_t                users;
};
```
- 구조체 멤버
    - next
        - sk_buff list next
    - prev
        - sk_buff list prev
    - sk
        - ...

---------------------------------------------------------
### API, 구조체... 등 작성할 제목 #1
```c
void test_function(void)
{
    printf("hello\n");
}
```
- 입력
    - 입력인자 #1 설명
    - 입력인자 #2 설명
- 리턴
    - 리턴값 설명
- 기능 설명
    - 해당 함수는 어쩌구...
---------------------------------------------------------
### API, 구조체... 등 작성할 제목 #1
```c
void test_function(void)
{
    printf("hello\n");
}
```
- 입력
    - 입력인자 #1 설명
    - 입력인자 #2 설명
- 리턴
    - 리턴값 설명
- 기능 설명
    - 해당 함수는 어쩌구...
---------------------------------------------------------
