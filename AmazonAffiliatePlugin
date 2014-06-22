if ($modx->event->name == 'OnWebPagePrerender')
{
    $content = $modx->documentOutput;

    // Collect all link tags on the page to $matches array
    preg_match_all(
        "/<a [^>]*?href=[\"\'](.*?)[\"\'][^>]*>.*?<\/a>/im",
        $content,
        $matches
    );

    if (!empty($matches[0]))
    {
        foreach ($matches[0] as $key => $tag) {
            // Get and parse the value of "href" attribute
            $url = trim($matches[1][$key]);
            $url_info = parse_url($url);
			
			// Skip non-amazon urls
			$host = isset($url_info['host']) ? strtolower($url_info['host']) : '';
			if (!in_array($host, array('amazon.com', 'www.amazon.com'))) {
				continue;
			}
			
			// Skip links which already have Associates ID
			if (isset($url_info['query'])) {
				$url_info['query'] = str_replace('&amp;', '&', $url_info['query']);
				parse_str($url_info['query'], $query);
				if (!empty($query['tag'])) {
					continue;
				}
			}

            // Add Associates ID to the link
			$new_url = $url_info['scheme'] . '://' . $url_info['host'];
			if (isset($url_info['path'])) {
				$new_url .= $url_info['path'];
			}
			$new_url = rtrim($new_url, '/') . '/?tag=' . $affiliate_id;
			if (isset($url_info['query'])) {
				$new_url .= '&' . $url_info['query'];
			}
			if (isset($url_info['fragment'])) {
				$new_url .= '#' . $url_info['fragment'];
			}
			$new_tag = preg_replace("/href=[\"\'](.*?)[\"\']/", 'href="' . $new_url . '"', $tag);
            $content = str_replace($tag, $new_tag, $content);
        }

        // Update content
        $modx->documentOutput = $content;
    }
}
