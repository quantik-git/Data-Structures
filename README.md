---


---

<h1 id="listas-ligadassimples">Listas Ligadas(Simples)</h1>
<h2 id="definição">Definição</h2>
<p>É uma estrutura de dados linear e dinâmica. Uma lista ligada é um conjunto de nós que por sua vez são compostos por dois membros. Uma parte que armazena os dados(neste caso <code>int valor</code>) e um apontador para o próximo elemento da lista.</p>
<pre><code>+---+---+    +---+---+    +---+------+
| 1 | p-|---&gt;| 2 | p-|---&gt;| 3 | NULL |
+---+---+    +---+---+    +---+------+
</code></pre>
<blockquote>
<p>Lista ligada com 3 nós no último nó temos um apontador para NULL para simpolizar o fim da lista</p>
</blockquote>
<p>Implementação de uma lista ligada em C:</p>
<pre class=" language-c"><code class="prism  language-c"><span class="token keyword">struct</span> No <span class="token punctuation">{</span>
	<span class="token keyword">int</span> valor<span class="token punctuation">;</span>
	<span class="token keyword">struct</span> No <span class="token operator">*</span>prox<span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token keyword">typedef</span> <span class="token keyword">struct</span> No <span class="token operator">*</span>LLint<span class="token punctuation">;</span>
</code></pre>
<p>Primeiro definimos a <code>struct No</code> que vai representar cada nó da lista e de seguida definimos <code>LLint</code> como um apontador para um <code>No</code> de forma a podermos representar uma lista vazia.</p>
<h2 id="operações">Operações</h2>
<p>Listas ligadas são bastante versáteis permitindo tanto inserção como eliminação de elementos no ínicio, no fim e no meio da lista.</p>
<h3 id="inserção-no-ínicio">Inserção no ínicio</h3>
<pre class=" language-c"><code class="prism  language-c"><span class="token comment">// Também denominado de "push"</span>
<span class="token keyword">void</span> <span class="token function">insert_front</span><span class="token punctuation">(</span>LLint <span class="token operator">*</span>lista<span class="token punctuation">,</span> <span class="token keyword">int</span> valor<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	LLint novo <span class="token operator">=</span> <span class="token function">malloc</span><span class="token punctuation">(</span><span class="token keyword">sizeof</span><span class="token punctuation">(</span><span class="token keyword">struct</span> LLint<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	novo<span class="token operator">-&gt;</span>valor <span class="token operator">=</span> valor<span class="token punctuation">;</span>
	novo<span class="token operator">-&gt;</span>prox <span class="token operator">=</span> <span class="token operator">*</span>lista<span class="token punctuation">;</span>
	<span class="token operator">*</span>lista <span class="token operator">=</span> novo<span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li>Alocamos espaço para o novo elemento</li>
<li>Atribuímos o valor</li>
<li>Apontamos o novo para a cabeça da lista</li>
<li>Atualizamos a referência à cabeça</li>
</ol>
<h3 id="inserção-no-fim">Inserção no fim</h3>
<pre class=" language-c"><code class="prism  language-c"><span class="token comment">// Nomenclatura alternativa "append"</span>
<span class="token keyword">void</span> <span class="token function">insert_end</span><span class="token punctuation">(</span>LLint <span class="token operator">*</span>lista<span class="token punctuation">,</span> <span class="token keyword">int</span> valor<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	LLint cursor <span class="token operator">=</span> <span class="token operator">*</span>lista<span class="token punctuation">;</span>
	LLint novo <span class="token operator">=</span> <span class="token function">malloc</span><span class="token punctuation">(</span><span class="token keyword">sizeof</span><span class="token punctuation">(</span><span class="token keyword">struct</span> LLint<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	novo<span class="token operator">-&gt;</span>valor <span class="token operator">=</span> valor<span class="token punctuation">;</span>
	novo<span class="token operator">-&gt;</span>prox <span class="token operator">=</span> <span class="token constant">NULL</span><span class="token punctuation">;</span>
	
	<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">*</span>lista <span class="token operator">=</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token operator">*</span>lista <span class="token operator">=</span> novo<span class="token punctuation">;</span>
		<span class="token keyword">return</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>

	<span class="token keyword">while</span><span class="token punctuation">(</span>cursor<span class="token operator">-&gt;</span>prox <span class="token operator">!=</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
		cursor <span class="token operator">=</span> cursor<span class="token operator">-&gt;</span>prox<span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
	cursor<span class="token operator">-&gt;</span>prox <span class="token operator">=</span> novo<span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li>Alocamos espaço para o novo elemento</li>
<li>Atribuímos o valor</li>
<li>Apontámos o <code>novo</code> para <code>NULL</code></li>
<li>Verificamos o caso de a lista ser vazia</li>
<li>Percorremos a lista até ao fim</li>
<li>Apontámos o fim da lista para o <code>novo</code></li>
</ol>
<h3 id="inserção-no-meio">Inserção no meio</h3>
<pre class=" language-c"><code class="prism  language-c"><span class="token keyword">void</span> <span class="token function">insert_at</span><span class="token punctuation">(</span>LLint <span class="token operator">*</span>lista<span class="token punctuation">,</span> <span class="token keyword">int</span> valor<span class="token punctuation">,</span> <span class="token keyword">int</span> indice<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
	LLint cursor <span class="token operator">=</span> <span class="token operator">*</span>lista<span class="token punctuation">;</span>  
	LLint anterior <span class="token operator">=</span> <span class="token constant">NULL</span><span class="token punctuation">;</span>  
	LLint novo <span class="token operator">=</span> <span class="token punctuation">(</span>LLint<span class="token punctuation">)</span> <span class="token function">malloc</span><span class="token punctuation">(</span><span class="token keyword">sizeof</span><span class="token punctuation">(</span><span class="token keyword">struct</span> LLint<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	novo<span class="token operator">-&gt;</span>valor <span class="token operator">=</span> valor<span class="token punctuation">;</span>  
	novo<span class="token operator">-&gt;</span>prox <span class="token operator">=</span> <span class="token constant">NULL</span><span class="token punctuation">;</span>  

	<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">*</span>lista <span class="token operator">==</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token operator">*</span>lista <span class="token operator">=</span> novo<span class="token punctuation">;</span>  
		<span class="token keyword">return</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>  

	<span class="token keyword">while</span><span class="token punctuation">(</span>cursor<span class="token operator">-&gt;</span>prox <span class="token operator">!=</span> <span class="token constant">NULL</span> <span class="token operator">&amp;&amp;</span> indice<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		indice<span class="token operator">--</span><span class="token punctuation">;</span>  
		anterior <span class="token operator">=</span> cursor<span class="token punctuation">;</span>  
		cursor <span class="token operator">=</span> cursor<span class="token operator">-&gt;</span>prox<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  

	novo<span class="token operator">-&gt;</span>prox <span class="token operator">=</span> cursor<span class="token punctuation">;</span>  

	<span class="token keyword">if</span> <span class="token punctuation">(</span>anterior <span class="token operator">==</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token operator">*</span>lista <span class="token operator">=</span> novo<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
		anterior<span class="token operator">-&gt;</span>prox <span class="token operator">=</span> novo<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li>Alocamos espaço para o novo elemento</li>
<li>Atribuímos o valor</li>
<li>Apontámos o <code>novo</code> para <code>NULL</code></li>
<li>Verificamos o caso de a lista ser vazia</li>
<li>Iteramos a lista até ao <code>indice</code> ou até ao fim</li>
<li>Apontámos o <code>novo</code> para o nó seguinte da lista</li>
<li>Verificamos o caso de o <code>indice</code> ser 0</li>
<li>Apontamos o nó anterior/cabeça para o <code>novo</code></li>
</ol>
<h3 id="eliminar-primeiro-nó">Eliminar primeiro nó</h3>
<pre class=" language-c"><code class="prism  language-c"><span class="token keyword">void</span> <span class="token function">delete_start</span><span class="token punctuation">(</span>LLint <span class="token operator">*</span>lista<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">*</span>lista <span class="token operator">==</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token keyword">return</span><span class="token punctuation">;</span>
	LLint temp <span class="token operator">=</span> <span class="token operator">*</span>lista<span class="token punctuation">;</span>
	<span class="token operator">*</span>lista <span class="token operator">=</span> temp<span class="token operator">-&gt;</span>prox<span class="token punctuation">;</span>
	<span class="token function">free</span><span class="token punctuation">(</span>temp<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li>Verificar se a lista é vazia</li>
<li>Inicializar apontador temporário com cabeça da lista</li>
<li>Atualizar referência da cabeça</li>
<li>Libertar memória do nó eliminado</li>
</ol>
<h3 id="eliminar-último-nó">Eliminar último nó</h3>
<pre class=" language-c"><code class="prism  language-c"><span class="token keyword">void</span> <span class="token function">delete_end</span><span class="token punctuation">(</span>LLint <span class="token operator">*</span>lista<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">*</span>lista <span class="token operator">==</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token keyword">return</span><span class="token punctuation">;</span>
	LLint cursor <span class="token operator">=</span> <span class="token operator">*</span>lista<span class="token punctuation">;</span>  
	LLint anterior <span class="token operator">=</span> <span class="token constant">NULL</span><span class="token punctuation">;</span>  

	<span class="token keyword">while</span><span class="token punctuation">(</span>cursor<span class="token operator">-&gt;</span>prox <span class="token operator">!=</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		anterior <span class="token operator">=</span> cursor<span class="token punctuation">;</span>  
		cursor <span class="token operator">=</span> cursor<span class="token operator">-&gt;</span>prox<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  

	<span class="token keyword">if</span> <span class="token punctuation">(</span>anterior <span class="token operator">==</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token operator">*</span>lista <span class="token operator">=</span> <span class="token constant">NULL</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
		anterior<span class="token operator">-&gt;</span>prox <span class="token operator">=</span> <span class="token constant">NULL</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li>Verificar se a lista é vazia</li>
<li>Alocar nó temporário e inicializa-lo cabeça da lista</li>
<li>Atualizar referência da cabeça</li>
<li>Libertar memória do nó eliminado</li>
</ol>
<h3 id="eliminar-nó-do-meio">Eliminar nó do meio</h3>
<pre class=" language-c"><code class="prism  language-c"><span class="token keyword">void</span> <span class="token function">delete_at</span><span class="token punctuation">(</span>LLint <span class="token operator">*</span>lista<span class="token punctuation">,</span> <span class="token keyword">int</span> indice<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">*</span>lista <span class="token operator">==</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token keyword">return</span><span class="token punctuation">;</span>
	LLint cursor <span class="token operator">=</span> <span class="token operator">*</span>lista<span class="token punctuation">;</span>  
	LLint anterior <span class="token operator">=</span> <span class="token constant">NULL</span><span class="token punctuation">;</span>  

	<span class="token keyword">while</span><span class="token punctuation">(</span>cursor<span class="token operator">-&gt;</span>prox <span class="token operator">!=</span> <span class="token constant">NULL</span> <span class="token operator">&amp;&amp;</span> indice<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		indice<span class="token operator">--</span><span class="token punctuation">;</span>  
		anterior <span class="token operator">=</span> cursor<span class="token punctuation">;</span>  
		cursor <span class="token operator">=</span> cursor<span class="token operator">-&gt;</span>prox<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  

	<span class="token keyword">if</span> <span class="token punctuation">(</span>indice<span class="token punctuation">)</span> <span class="token keyword">return</span><span class="token punctuation">;</span> <span class="token comment">// se o indice exceder o comprimento da lista</span>

	<span class="token keyword">if</span> <span class="token punctuation">(</span>anterior <span class="token operator">==</span> <span class="token constant">NULL</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token operator">*</span>lista <span class="token operator">=</span> cursor<span class="token operator">-&gt;</span>prox<span class="token punctuation">;</span>
	<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
		anterior<span class="token operator">-&gt;</span>prox <span class="token operator">=</span> cursor<span class="token operator">-&gt;</span>prox<span class="token punctuation">;</span>
	<span class="token punctuation">}</span>

	<span class="token function">free</span><span class="token punctuation">(</span>cursor<span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
<h2 id="complexidade-das-operações">Complexidade das operações</h2>
<ul>
<li>
<p>Inserção</p>
<ul>
<li>Cabeça O(1)</li>
<li>Cauda O(n) (O(1) quando se tem uma referência pro fim da lista)</li>
<li>Meio O(n)</li>
</ul>
</li>
<li>
<p>Eliminação</p>
<ul>
<li>Cabeça O(1)</li>
<li>Cauda O(n) (O(1) quando se tem uma referência pro fim da lista)</li>
<li>Meio O(n)</li>
</ul>
</li>
</ul>
<h2 id="uml-diagrams">UML diagrams</h2>
<p>You can render UML diagrams using <a href="https://mermaidjs.github.io/">Mermaid</a>. For example, this will produce a sequence diagram:</p>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-TT500qtFjNqgjL0F" width="100%" style="max-width: 500.9549789428711px;" viewBox="0 0 500.9549789428711 172.59999084472656"><g transform="translate(-12, -12)"><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M120.4875176804892,78.23333358764648L179.31665802001953,49.94166564941406L255.2583236694336,49.94166564941406" marker-end="url(#arrowhead87)" style="fill:none"></path><defs><marker id="arrowhead87" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M120.4875176804892,124.94999313354492L179.31665802001953,153.24166107177734L234.79998779296875,153.24166107177734" marker-end="url(#arrowhead88)" style="fill:none"></path><defs><marker id="arrowhead88" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M315.1416549682617,49.94166564941406L360.59999084472656,49.94166564941406L408.70982831217043,79.48182589315182" marker-end="url(#arrowhead89)" style="fill:none"></path><defs><marker id="arrowhead89" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M335.59999084472656,153.24166107177734L360.59999084472656,153.24166107177734L408.70982831217043,124.7015008280396" marker-end="url(#arrowhead90)" style="fill:none"></path><defs><marker id="arrowhead90" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" style="opacity: 1;" transform="translate(179.31665802001953,49.94166564941406)"><g transform="translate(-30.48332977294922,-13.358329772949219)" class="label"><foreignObject width="60.96665954589844" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">Link text</span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" style="opacity: 1;" id="A" transform="translate(71.91666412353516,101.5916633605957)"><rect rx="0" ry="0" x="-51.916664123535156" y="-23.35832977294922" width="103.83332824707031" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-41.916664123535156,-13.358329772949219)"><foreignObject width="83.83332824707031" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Square Rect</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B" transform="translate(285.19998931884766,49.94166564941406)"><circle x="-29.941665649414062" y="-23.35832977294922" r="29.941665649414062"></circle><g class="label" transform="translate(0,0)"><g transform="translate(-19.941665649414062,-13.358329772949219)"><foreignObject width="39.883331298828125" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Circle</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C" transform="translate(285.19998931884766,153.24166107177734)"><rect rx="5" ry="5" x="-50.400001525878906" y="-23.35832977294922" width="100.80000305175781" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-40.400001525878906,-13.358329772949219)"><foreignObject width="80.80000305175781" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Round Rect</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="D" transform="translate(445.2774848937988,101.5916633605957)"><polygon points="59.677494049072266,0 119.35498809814453,-59.677494049072266 59.677494049072266,-119.35498809814453 0,-59.677494049072266" rx="5" ry="5" transform="translate(-59.677494049072266,59.677494049072266)"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-32.94999694824219,-13.358329772949219)"><foreignObject width="65.89999389648438" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Rhombus</div></foreignObject></g></g></g></g></g></g></svg></div>

