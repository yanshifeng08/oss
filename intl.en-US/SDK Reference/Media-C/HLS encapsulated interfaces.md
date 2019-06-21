# HLS encapsulated interfaces {#concept_32165_zh .concept}

The OSS MEDIA C SDK client can pack the received H.264 or AAC format data into the TS or M3U8 format and then write the data into the OSS. Apart from basic interfaces, the SDK also provides encapsulated recording and live video interfaces.

## Interface {#section_sk3_wzq_lfb .section}

HLS-related encapsulated interfaces are all located in the oss\_media\_hls\_stream.h. Currently the following interfaces are provided:

-   oss\_media\_hls\_stream\_open
-   oss\_media\_hls\_stream\_write
-   oss\_media\_hls\_stream\_close

The functions and notes of usage of various interfaces are mentioned as follows:

-   Basic structure

    ``` {#codeblock_tmq_02h_1e9 .language-c}
    /**
     * Information about OSS MEDIA HLS STREAM OPTIONS
     */
    typedef struct oss_media_hls_stream_options_s {
        int8_t is_live;
        char *bucket_name;
        char *ts_name_prefix;
        char *m3u8_name;
        int32_t video_frame_rate;
        int32_t audio_sample_rate;
        int32_t hls_time;
        int32_t hls_list_size;
    } oss_media_hls_stream_options_t;
    
    /**
     * Information about OSS MEDIA HLS STREAM
     */
    typedef struct oss_media_hls_stream_s {
        const oss_media_hls_stream_options_t *options;
        oss_media_hls_file_t *ts_file;
        oss_media_hls_file_t *m3u8_file;
        oss_media_hls_frame_t *video_frame;
        oss_media_hls_frame_t *audio_frame;
        oss_media_hls_m3u8_info_t *m3u8_infos;
        int32_t ts_file_index;
        int64_t current_file_begin_pts;
        int32_t has_aud;
        aos_pool_t *pool;
    } oss_media_hls_stream_t;
    					
    ```

    **Note:** 

    -   is\_live: Indicates whether the video is in live mode. When the video is in live mode, the M3U8 object only contains the information of the last few TS objects.
    -   bucket\_name: Indicates the names of the buckets that store the HLS files.
    -   ts\_name\_prefix: Indicates the prefix of the TS object name.
    -   m3u8\_name: Indicates the name of the M3U8 object.
    -   video\_frame\_rate: Indicates the frame rate of the video data.
    -   audio\_sample\_rate: Indicates the sample rate of the audio data.
    -   hls\_time: Indiicates the maximum duration of each TS object.
    -   hls\_list\_size: Indicates the number of TS objects retained in the M3U8 object in live video mode.
-   Open an HLS stream object

    ``` {#codeblock_cqf_8cg_143 .language-c}
    /**
     *  @brief  Open an OSS HLS file
     *  @param[in]  auth_func   The authorization function to set access_key_id/access_key_secret
     *  @param[in]  options     Configuration information
     *  @return:
     *      If NULL is returned, it indicates success. Otherwise, it indicates failure.
     */
    oss_media_hls_stream_t* oss_media_hls_stream_open(auth_fn_t auth_func,
                            const oss_media_hls_stream_options_t *options);
    					
    ```

    **Note:** 

    -   For the complete example code, see [GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_stream_sample.c)
    -   .
-   Close an HLS stream object

    ``` {#codeblock_9go_emc_sav .language-c}
    /**
     *  @brief  Close the HLS stream object
     */
    int oss_media_hls_stream_close(oss_media_hls_stream_t *stream);
    
    					
    ```

    **Note:** For the complete example code, see [GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_stream_sample.c).

-   Write data into an HLS stream object

    ``` {#codeblock_cfg_60a_4s1 .language-c}
    /**
     *  @brief  Write the video and audio data
     *  @param[in]  video_buf Video data
     *  @param[in]  video_len Video data length. It can be 0
     *  @param[in]  audio_buf Audio data
     *  @param[in]  audio_len Audio data length. It can be 0
     *  @param[in]  stream    HLS stream
     *  @return:
     *      If 0 is returned, it indicates the operation is successful.
     *      Otherwise, it indicates an error has occurred
     */
    int oss_media_hls_stream_write(uint8_t *video_buf,
                                   uint64_t video_len,
                                   uint8_t *audio_buf,
                                   uint64_t audio_len,
                                   oss_media_hls_stream_t *stream);
    					
    ```

    Example project:

    ``` {#codeblock_x35_ttv_wgh .language-c}
    static void write_video_audio_vod() {
        int ret;
        int max_size = 10 * 1024 * 1024;
        FILE *h264_file, *aac_file;
        uint8_t *h264_buf, *aac_buf;
        int h264_len, aac_len;
    
        oss_media_hls_stream_options_t options;
        oss_media_hls_stream_t *stream;
    
        /* Set the HLS stream parameter value */
        options.is_live = 0;
        options.bucket_name = "<your buckete name>";
        options.ts_name_prefix = "vod/video_audio/test";
        options.m3u8_name = "vod/video_audio/vod.m3u8";
        options.video_frame_rate = 30;
        options.audio_sample_rate = 24000;
        options.hls_time = 5;
    
        /* Open the HLS stream */
        stream = oss_media_hls_stream_open(auth_func, &options);
        if (stream == NULL) {
            printf("open hls stream failed.\n");
            return;
        }
    
        /* Create two buffers to store the audio and video data */
        h264_buf = malloc(max_size);
        aac_buf = malloc(max_size);
    
        /* Read a piece of video data and audio data and call the interface to write data to the OSS */
        {
            h264_file = fopen("/path/to/video/1.h264", "r");
            h264_len = fread(h264_buf, 1, max_size, h264_file);
            fclose(h264_file);
    
            aac_file = fopen("/path/to/audio/1.aac", "r");
            aac_len = fread(aac_buf, 1, max_size, aac_file);
            fclose(aac_file);
    
            ret = oss_media_hls_stream_write(h264_buf, h264_len, 
                    aac_buf, aac_len, stream);
            if (ret ! = 0) {
                printf("write vod stream failed.\n");
                return;
            }
        }
    
        /* Read a piece of video data and audio data again and call the interface to write data to the OSS */
        {
            h264_file = fopen("/path/to/video/2.h264", "r");
            h264_len = fread(h264_buf, 1, max_size, h264_file);
            fclose(h264_file);
    
            aac_file = fopen("/path/to/audio/1.aac", "r");
            aac_len = fread(aac_buf, 1, max_size, aac_file);
            fclose(aac_file);
    
            ret = oss_media_hls_stream_write(h264_buf, h264_len, 
                    aac_buf, aac_len, stream);
            if (ret ! = 0) {
                printf("write vod stream failed.\n");
                return;
            }
        }   
    
        /* After the data write, close the HLS stream */
        ret = oss_media_hls_stream_close(stream);
        if (ret ! = 0) {
            printf("close vod stream failed.\n");
            return;
        }
    
        /* Release the resources */
        free(h264_buf);
        free(aac_buf);
    
        printf("convert H. 264 and aac to HLS vod succeeded\n");
    }
    					
    ```

    **Note:** 

    -   Currently the recording and live video interfaces support video-only, audio-only and audio-video modes.
    -   For the complete example code, see [GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_stream_sample.c).
    -   Recording and live video interfaces are still relatively basic. If you need advanced features, use the basic interfaces to simulate the two interfaces to implement advanced custom features by yourself.
    -   You can view the effect in the example project.

