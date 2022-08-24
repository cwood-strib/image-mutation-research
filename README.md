# Image property DOM mutation research 

This repo collects research on how mutating `src` and `srcset` properties on the `img` tag affects image download behavior in browsers. 

## Definitions:

- `dynamic`: The attribute does not exist in HTML when the `DOMContentLoaded` event fires. It is added via JavaScript.
- `delayed`: The attribute does not exist in HTML when the `DOMContentLoaded` event fires. It is added via JavaScript after a delay using settimeout (to force property addition to a future next event loop tick).

## Testing

An html file for each case exists in the src directory. You can test results yourself on a local server with:

```shell
cd src/ 
php -S localhost:8000
```

Then observe image downloads from your browser network panel in dev tools.

## Results

### only src set

- Chrome: Works as expected
- Firefox: Works as expected
- Safari: Works as expected

### only srcset

- Chrome: Works as expected
- Firefox: Works as expected
- Safari: Works as expected

### src and srcset

- Chrome: Correct srcset is loaded 
- Firefox: Correct srcset is loaded 
- Safari: Works as expected

### srcset, then dynamic src

- Chrome: Does not cause extra download
- Firefox: Does not cause extra download
- Safari: Does not cause extra download

### src, then dynamic srcset

- Chrome: Src will download. Will trigger another download if the srcset solution is different than src.
- Firefox: Src will download. Will trigger another download if the srcset solution is different than src.
- Safari: Src will download. Will trigger another download if the srcset solution is different than src.

### src, then delayed dynamic srcset

- Chrome: Src will download. Will trigger download of srcset variant when set.
- Firefox:  Src will download. Will trigger download of srcset variant when set.
- Safari:  Src will download. Will trigger download of srcset variant when set.

### srcset, then delayed dynamic src

- Chrome: Re-triggers download of srcset
- Firefox: Re-triggers download of srcset image
- Safari: Does not re-trigger download

### dynamic src, then dynamic srcset

- Chrome: Downloads only correct srcset
- Firefox: Downloads only correct srcset
- Safari: Downloads src then downloads srcset

### dynamic srcset, then dynamic src

- Chrome: Downloads correct srcset
- Firefox: Downloads correct srcset
- Safari: Downloads correct srcset

### dynamic src, delayed srcset 

- Chrome: Src image will download. Then srcset will download.
- Firefox: Src is downloaded. Srcset is then downloaded.
- Safari: Src is downloaded. Srcset is then downloaded.

### dynamic srcset, delayd dynamic src

- Chrome: Will trigger re-download of srcset if delay is long enough. In local testing, a delay of 6 milliseconds will do.
- Firefox: Will trigger re-download of srcset if delay is long enough. In local testing, a delay of 6 milliseconds will do.
- Safari: Loads correct srcset image. Ignore src.