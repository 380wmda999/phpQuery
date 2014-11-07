<pre style="word-wrap: break-word; white-space: pre-wrap;">fix phpQuery encoding problem on phpQuery/DomDocumentWrapper.php  on line 362 - 385 
fix the method like this :
	/**
	 * Repositions meta[type=charset] at the start of head. Bypasses DOMDocument bug.
	 *
	 * @link http://code.google.com/p/phpquery/issues/detail?id=80
	 * @param $html
	 */
	protected function charsetFixHTML($markup) {
		$matches = array();
		// find meta tag
		preg_match('@\s*&lt;meta[^&gt;]+http-equiv\\s*=\\s*(["|\'])Content-Type\\1([^&gt;]+?)&gt;@i',
			$markup, $matches, PREG_OFFSET_CAPTURE
		);
		if (! isset($matches[0]))
			return;
		$metaContentType = $matches[0][0];
		$markup = substr($markup, 0, $matches[0][1])
			.substr($markup, $matches[0][1]+strlen($metaContentType));
                preg_match('#&lt;.*?head[^&gt;]*&gt;#', $markup,$headMatches,PREG_OFFSET_CAPTURE);
		$headStart = $headMatches[0][1];
                $headLen = strlen($headMatches[0][0]);
		$markup = substr($markup, 0, $headStart+ $headLen).$metaContentType
			.substr($markup, $headStart+$headLen);
		return $markup;
	}</pre>
