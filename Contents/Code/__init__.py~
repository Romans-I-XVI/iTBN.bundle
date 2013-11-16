import re

TITLE = 'iTBN'

ART = 'art-default.jpg'
ICON = 'icon-default.png'

BASE_URL = 'http://www.itbn.org/'

###################################################################################################

def Start():
  Plugin.AddPrefixHandler('/video/iTBN', MainMenu, TITLE, ICON, ART)
  Plugin.AddViewGroup('List', viewMode = 'List', mediaType = 'items')

  # Set the default ObjectContainer attributes
  ObjectContainer.title1 = TITLE
  ObjectContainer.view_group = 'List'
  ObjectContainer.art = R(ART)
  ObjectContainer.no_history = True

  # Default icons for DirectoryObject and VideoClipObject in case there isn't an image
  DirectoryObject.thumb = R(ICON)
  DirectoryObject.art = R(ART)
  VideoClipObject.thumb = R(ICON)
  VideoClipObject.art = R(ART)

  # Set the default cache time
  HTTP.CacheTime = CACHE_1HOUR

###################################################################################################

def MainMenu():
  oc = ObjectContainer(no_history=True)
  oc.add(DirectoryObject(key = Callback(Recent), title = 'Recent'))
  oc.add(DirectoryObject(key = Callback(Categories), title = 'Categories'))
  oc.add(DirectoryObject(key = Callback(Programs), title = 'Programs'))
  oc.add(DirectoryObject(key = Callback(Movies), title = 'Movies'))
  oc.add(DirectoryObject(key = Callback(Live), title = 'Live'))
  return oc




###################################################################################################

def Recent():
	oc = ObjectContainer(no_history=True)
	link = HTTP.Request('http://www.tbn.org/watch/mobile_app/v3/itbnapi.php?platform=android&request_path=%2Fapi%2Fv1.0%2Fvideos%2Flimit%2F250%2Fsortby%2Fairdate&device_name=GT-I9100&os_ver=2.3.4&screen_width=1600&screen_height=900&app_ver=3.0&UUID=1d5f5000-656a-4a16-847f-138937d4d0c4').content
	match=re.compile('"embedCode":"(.+?)"').findall(link)
	title=re.compile('"displayTitle":"(.+?)"').findall(link)
	date=re.compile('"airDate":"(.+?) 00:00:00"').findall(link)
	description=re.compile('\"description\":\"(.+?)"air').findall(link)
	duration=re.compile('\"length\":\"(.+?)\",').findall(link)
	name = title
	thumbnail=re.compile('"thumbnailUrl":"(.+?)"').findall(link)
	thumbnail = [w.replace('\\', '') for w in thumbnail]
	prefixcode='http://www.tbn.org/watch/mobile_app/v3/ooyala_strip_formats.php?method=GET&key=undefined&secret=undefined&expires=600&embedcode=',len(match)
	prefix=[prefixcode[0]]*prefixcode[1]
	suffixcode='&requestbody=&parameters=',len(match)
	suffix=[suffixcode[0]]*suffixcode[1]
	source=zip((prefix),(match),(suffix))
	mylist=zip((source),(name),(thumbnail),(description),(date),(duration))
	for url,name,thumbnail,description,date,duration in mylist:
		description=description.replace("\\","")
		description=description.replace("u2019","\'")
		description=description.split('\"', 1)[-1]
		description=description.replace('\"','')
		description=description.replace(',','')
		duration=int(duration)
		name = reduce(lambda rst, d: rst * 1 + d, (name))
		url = reduce(lambda rst, d: rst * 1 + d, (url))
		oc.add(VideoClipObject(
		key = Callback(GETSOURCE, url = url),
		rating_key = url,
		title = name +' - '+ description +' ('+date+')',
		thumb = thumbnail,
		duration = duration,
		items = [
			MediaObject(
			container = Container.MP4,
			video_codec = VideoCodec.H264,
			audio_codec = AudioCodec.AAC,
			audio_channels = 2,
			parts = [PartObject(key = Callback(GETSOURCE, url = url))]
			)
		]
		))
	return oc
###################################################################################################

def Categories():
	oc = ObjectContainer()
	oc.add(DirectoryObject(key = Callback(FaithIssues), title = 'Faith Issues'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Classics'), title = 'Classics'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Educational'), title = 'Educational'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Health+%26+Fitness'), title = 'Health & Fitness'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Kids'), title = 'Kids'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Music'), title = 'Music'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Specials'), title = 'Specials'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Teens'), title = 'Teens'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Documentaries'), title = 'Documentaries'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Family+%26+Variety'), title = 'Family & Variety'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Holidays'), title = 'Holidays'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Programs/sublib/Reality'), title = 'Reality'))
	return oc
###################################################################################################

def FaithIssues():
	oc = ObjectContainer()
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Angels'), title = 'Angels'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Crisis'), title = 'Crisis'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Depression'), title = 'Depression'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/End+Times'), title = 'End Times'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Faith'), title = 'Faith'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Family'), title = 'Family'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Finances'), title = 'Finances'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Heaven'), title = 'Heaven'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Hell'), title = 'Hell'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Loss'), title = 'Loss'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Marriage'), title = 'Marriage'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Prayer'), title = 'Prayer'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Purpose'), title = 'Purpose'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Salvation'), title = 'Salvation'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Satan'), title = 'Satan'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Sexuality'), title = 'Sexuality'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Suicide'), title = 'Suicide'))
	oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org/index/subview/lib/Faith+Issues/sublib/Teen+Issues'), title = 'Teen Issues'))
	return oc
###################################################################################################

def Programs():
	oc = ObjectContainer(no_history=True)
	page_content = HTTP.Request('http://www.itbn.org/programs').content
	match=re.compile('									<a href="(.+?)">(.+?)</a>').findall(page_content)
	for url,name in match:
		oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org' + url), title = name))
	return oc
	

###################################################################################################

def Movies():
	oc = ObjectContainer()
	link = HTTP.Request('http://www.tbn.org/watch/mobile_app/v3/itbnapi.php?platform=android&request_path=%2Fapi%2Fv1.0%2Fvideos%2Flimit%2F250%2Fsortby%2Fairdate%2Fcategory%2F1723&device_name=GT-I9100&os_ver=2.3.4&screen_width=1600&screen_height=900&app_ver=3.0&UUID=1d5f5000-656a-4a16-847f-138937d4d0c4').content
	match=re.compile('"embedCode":"(.+?)"').findall(link)
	title=re.compile('"displayTitle":"(.+?)"').findall(link)
	date=re.compile('"airDate":"(.+?) 00:00:00"').findall(link)
	description=re.compile('\"description\":\"(.+?)\","air').findall(link)
	name=title
	thumbnail=re.compile('"thumbnailUrl":"(.+?)"').findall(link)
	thumbnail = [w.replace('\\', '') for w in thumbnail]
	mylist=zip((match),(name),(thumbnail),(description),(date))
	for url,name,thumbnail,description,date in mylist:
		description=description.replace("\\","")
		description=description.replace("u2019","\'")
		prefixcode='http://www.tbn.org/watch/mobile_app/v3/ooyala_strip_formats.php?method=GET&key=undefined&secret=undefined&expires=600&embedcode='
		suffixcode='&requestbody=&parameters='
		url=prefixcode+url+suffixcode
		Log(url)
		oc.add(VideoClipObject(
		key = Callback(GETSOURCE, url = url),
		rating_key = url,
		title = name +' - '+ description +' ('+date+')',
		thumb = thumbnail,
		items = [
			MediaObject(
			container = Container.MP4,
			video_codec = VideoCodec.H264,
			audio_codec = AudioCodec.AAC,
			audio_channels = 2,
			parts = [PartObject(key = Callback(GETSOURCE, url = url))]
			)
		]
		))
	return oc
                
###################################################################################################

def Live():
	oc = ObjectContainer()
	link = HTTP.Request('http://www.tbn.org/watch/mobile_app/v3/getlivestreams.php?device_name=GT-I9100&os_ver=2.3.4&screen_width=1600&screen_height=900&app_ver=3.0&UUID=1d5f5000-656a-4a16-847f-138937d4d0c4').content
	#if settings.getSetting("livequality") == '0':
	#	match=re.compile('\"andsuperstreamurl\":\"(.+?)\"').findall(link)
	#if settings.getSetting("livequality") == '1':
	#	match='rtmp://cp114430.live.edgefcs.net/live/ playpath=tbn_mbr_300@101613 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114428.live.edgefcs.net/live/ playpath=churchch_mbr_300@101620 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114432.live.edgefcs.net/live/ playpath=jctv_mbr_300@101615 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114426.live.edgefcs.net/live/ playpath=soac_mbr_300@101622 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114434.live.edgefcs.net/live/ playpath=enlace_mbr_300@101618 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114436.live.edgefcs.net/live/ playpath=enlacejuvenil_800@102106 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp129063.live.edgefcs.net/live/ playpath=nejat_mbr_300@101623 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp129064.live.edgefcs.net/live/ playpath=healing_mbr_300@101624 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp129065.live.edgefcs.net/live/ playpath=tbnrussia-high@58776 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp129066.live.edgefcs.net/live/ playpath=soacrussia-high@58777 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://mediaplatform2.trinetsolutions.com/tbn/ playpath=juce_super.sdp  live=true','rtmp://mediaplatform2.trinetsolutions.com/tbn_repeater/ playpath=tbnafrica.stream live=true'
#
	#if settings.getSetting("livequality") == '2':
	match='rtmp://cp114430.live.edgefcs.net/live/ playpath=tbn_mbr_600@101613 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114428.live.edgefcs.net/live/ playpath=churchch_mbr_600@101620 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114432.live.edgefcs.net/live/ playpath=jctv_mbr_600@101615 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114426.live.edgefcs.net/live/ playpath=soac_mbr_600@101622 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114434.live.edgefcs.net/live/ playpath=enlace_mbr_600@101618 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp114436.live.edgefcs.net/live/ playpath=enlacejuvenil_800@102106 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp129063.live.edgefcs.net/live/ playpath=nejat_mbr_600@101623 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp129064.live.edgefcs.net/live/ playpath=healing_mbr_600@101624 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp129065.live.edgefcs.net/live/ playpath=tbnrussia-high@58776 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://cp129066.live.edgefcs.net/live/ playpath=soacrussia-high@58777 pageURL=http://www.tbn.org/watch-us swfUrl=http://players.edgesuite.net/flash/plugins/osmf/advanced-streaming-plugin/v2.11/osmf2.0/AkamaiAdvancedStreamingPlugin.swf swfVfy=true live=true','rtmp://mediaplatform2.trinetsolutions.com/tbn/ playpath=juce_super.sdp  live=true','rtmp://mediaplatform2.trinetsolutions.com/tbn_repeater/ playpath=tbnafrica.stream live=true'
   title=re.compile('\"name\":\"(.+?)\"').findall(link)
   thumbnail=re.compile('\"icon\":\"(.+?)\"').findall(link)
   mylist=zip((match),(title),(thumbnail))
   for url,name,thumbnail in mylist:
		oc.add(VideoClipObject(
		key = url,
		rating_key = url,
		title = name +' - '+ description +' ('+date+')',
		thumb = thumbnail,
		items = [
			MediaObject(
			container = Container.MP4,
			video_codec = VideoCodec.H264,
			audio_codec = AudioCodec.AAC,
			audio_channels = 2,
			parts = [PartObject(key = url)]
			)
		]
		))
	return oc


###################################################################################################

def Links(url):
	oc = ObjectContainer(no_history=True)
	link = HTTP.Request(url).content
	match=re.compile('						<a href=".+?/ec/(.+?)"><img src=".+?" alt=".+?" title=".+?"').findall(link)
	title=re.compile('						<a href=".+?"><img src=".+?" alt=".+?" title="(.+?)"').findall(link)
	function=' '*len(match)
	leftparentheses='('*len(match)
	date=re.compile('<span class="air_date">(.+?)</span>').findall(link)
	description=re.compile('						<span class="description">(.+?)</span>').findall(link)
	rightparentheses=')'*len(match)
	name=title
	thumbnail=re.compile('						<a href=".+?"><img src="(.+?)" alt=".+?" title=".+?"').findall(link)
	thumbnail = [w.replace('\\', '') for w in thumbnail]
	prefixcode='http://www.tbn.org/watch/mobile_app/v3/ooyala_strip_formats.php?method=GET&key=undefined&secret=undefined&expires=600&embedcode=',len(match)
	prefix=[prefixcode[0]]*prefixcode[1]
	suffixcode='&requestbody=&parameters=',len(match)
	suffix=[suffixcode[0]]*suffixcode[1]
	nextpage=re.compile('<div class=\'btn_container\'><a href=\'(.+?)\' class=\'btn_next\'>').findall(link)
	nextpagelabelurl=re.compile('<div class=\'btn_container\'><a href=\'.+?/page/(.+?.+?.+?)').findall(link)
	airdatepage=re.compile('.+?airDate=(.+?.+?.+?.+?.+?.+?.+?.+?.+?.+?)').findall(url)
	if nextpage:
		nextpagelabel=nextpagelabelurl[0]
		nextpagelabel=nextpagelabel.replace('%27','')
		nextpagelabel=nextpagelabel.replace('\'','')
		if airdatepage:
			nextpagelabel=nextpagelabel.replace(airdatepage[0],'')
			nextpagelabel=[w.replace('/', '') for w in nextpagelabel]
			nextpagelabel=" ".join(nextpagelabel)
			nextpagelabel=[int(s) for s in nextpagelabel.split() if s.isdigit()]
			nextpagelabel=''.join(str(e) for e in nextpagelabel)
	previouspage=re.compile('class=\'btn_first\'>&lt;&lt;</a></li><li><a href=\'(.+?)\' class=\'btn_prev\'>').findall(link)
	previouspagelabelurl=re.compile('class=\'btn_first\'>&lt;&lt;</a></li><li><a href=\'.+?/page/(.+?.+?.+?)').findall(link)
	if previouspage:
		previouspagelabel=previouspagelabelurl[0]
		previouspagelabel=previouspagelabel.replace('%27','')
		if airdatepage:
			previouspagelabel=previouspagelabel.replace(airdatepage[0],'')
			previouspagelabel=[w.replace('/', '') for w in previouspagelabel]
			previouspagelabel=" ".join(previouspagelabel)
			previouspagelabel=[int(s) for s in previouspagelabel.split() if s.isdigit()]
			previouspagelabel=''.join(str(e) for e in previouspagelabel)
	source=zip((prefix),(match),(suffix))
	mylist=zip((source),(name),(thumbnail),(description),(date))
	#if previouspage:
	#	previousurl = sys.argv[0]+"?url="+urllib.quote_plus('http://www.itbn.org'+previouspage[0])+"&mode="+str(11)+"&name="+urllib.quote_plus('Page '+previouspagelabel)                
	#	oc.add(DirectoryObject(key = previousurl, title = 'Page '+previouspagelabe))
		#addLink('Page '+previouspagelabel,previousurl,previous_thumb)
	for url,name,thumbnail,description,date in mylist:
		description=description.replace("&quot;","\"")
		description=description.replace("&#039;","\'")
		description=description.replace("&hellip;","...")
		description=description.replace("&amp;","&")
		description=description.split('\"', 1)[-1]
		description=description.replace('\"','')
		description=description.replace(',','')
		name=reduce(lambda rst, d: rst * 1 + d, (name))
		name=name.replace("&quot;","\"")
		name=name.replace("&#039;","\'")
		name=name.replace("&hellip;","...")
		name=name.replace("&amp;","&")
		url=reduce(lambda rst, d: rst * 1 + d, (url))
		oc.add(VideoClipObject(
		key = Callback(GETSOURCE, url = url),
		rating_key = url,
		title = name +' - '+ description +' ('+date+')',
		thumb = thumbnail,
		items = [
			MediaObject(
			container = Container.MP4,
			video_codec = VideoCodec.H264,
			audio_codec = AudioCodec.AAC,
			audio_channels = 2,
			parts = [PartObject(key = Callback(GETSOURCE, url = url))]
			)
		]
		))
	if nextpage:
		oc.add(DirectoryObject(key = Callback(Links, url = 'http://www.itbn.org'+nextpage[0]), title = 'Page '+nextpagelabel))
		# addDir('Page '+nextpagelabel,'http://www.itbn.org'+nextpage[0],1,next_thumb)
	return oc
	
################################################################################
@indirect
def GETSOURCE(url):
	link=HTTP.Request(url).content
	url=re.compile('"is_source":true,"file_size":.+?,"audio_codec":".+?","video_codec":".+?","average_video_bitrate":.+?,"stream_type":"single","url":"(.+?)"').findall(link)
	url = [w.replace('\\', '') for w in url]
	url = url[0]
	return IndirectResponse(VideoClipObject, key = url)