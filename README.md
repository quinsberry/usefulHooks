# Useful hooks


### Global
<details>
<summary><b>useInput</b></summary>
<div>
  <p>

    import { ChangeEvent, useState } from 'react'

    interface UseInputOutput {
        value: any
        onChange: (e: ChangeEvent<HTMLInputElement>) => void
    }
    
    export const useInput = (initialValue: any): useInputOutput => {

        const [value, setValue] = useState(initialValue)

        const onChange = (e: ChangeEvent<HTMLInputElement>) => setValue(e.target.value)

        return { value, onChange }
    }
    
  </p>
</div>
</details>

<details>
<summary><b>useHover</b></summary>
<div>
  <p>

    import { useEffect, useState } from "react"

    export const useHover = (ref): boolean => {

        const [isHovering, setHovering] = useState(false)

        const on = () => setHovering(true)
        const off = () => setHovering(false)

        useEffect(() => {
            if (!ref.current) {
                return;
            }
            const node = ref.current
    
            node.addEventListener('mouseenter', on)
            node.addEventListener('mousemove', on)
            node.addEventListener('mouseleave', off)
    
            return () => {
                node.removeEventListener('mouseenter', on)
                node.removeEventListener('mousemove', on)
                node.removeEventListener('mouseleave', off)
            }

        }, [])

        return isHovering
    }
  </p>
</div>
</details>

<details>
<summary><b>useScroll</b></summary>
<div>

  <p>

    import { useEffect, useRef } from "react"

    export const useScroll = (parentRef, childRef, callback): void => {

        const observer = useRef<IntersectionObserver>()

        useEffect(() => {
            const options = {
                root: parentRef.current,
                rootMargin: '0px',
                threshold: 0
            }
            observer.current = new IntersectionObserver(([target]) => {
                if (target.isIntersecting) {
                    callback()
                }
            }, options)
    
            observer.current.observe(childRef.current)
    
            return () => {
                observer.current.unobserve(childRef.current)
            }

        }, [callback])
    }
  </p>
</div>
</details>

<details>
<summary><b>useDebounce</b></summary>
<div>

  <p>

    import { useCallback, useRef } from "react"

    type Callback = (...args: any) => void

    export const useDebounce = (callback: Callback, delay: number): Callback => {

        const timer = useRef<number>()
    
        const debouncedCallback = useCallback((...args) => {
    
            if (timer.current) clearTimeout(timer.current)
    
            timer.current = setTimeout(() => {
                callback(...args)
            }, delay) as unknown as number

        }, [callback, delay])
    
        return debouncedCallback
    }
  </p>
</div>
</details>

<details>
<summary><b>useRequest</b></summary>
<div>

  <p>

    import { useEffect, useState } from "react"

    type UseRequestOutput<D> = [D, boolean, string]

    export const useRequest = <D = any>(request): UseRequestOutput<D> => {

        const [data, setData] = useState<D>(null);
        const [loading, setLoading] = useState(false);
        const [error, setError] = useState('');
    
        useEffect(() => {
            setLoading(true)
            request()
                .then(response => setData(response.data))
                .catch(error => setError(error))
                .finally(() => setLoading(false))
        }, [])
    
        return [data, loading, error]
    }
  </p>
</div>
</details>

### Redux
<details>
<summary><b>useTypedSelector</b></summary>
<div>
  <p>

    /*
    * RootState is a type of your root reducer.
    * Example: type RootState = ReturnType<typeof rootReducer>
    */

    import { TypedUseSelectorHook, useSelector } from 'react-redux'

    export const useTypedSelector: TypedUseSelectorHook<RootState> = useSelector


  </p>
</div>
</details>

<details>
<summary><b>useActions</b></summary>
<div>
  <p>

    /*
    * actionCreators is an object with all yours actions
    */

    import { useDispatch } from 'react-redux'
    import { bindActionCreators } from 'redux'
    
    export const useActions = () => {
        const dispatch = useDispatch()
        return bindActionCreators(actionCreators, dispatch)
    }

  </p>
</div>
</details>