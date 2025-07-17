```
const parseExpression = (exprString) => {
  // Удаляем все пробелы для упрощения парсинга
  const expr = exprString.replace(/\s/g, '');
  
  // Рекурсивная функция парсинга
  const parse = (subExpr) => {
    // Базовый случай - простое значение
    if (!subExpr.includes('&&') && !subExpr.includes('||')) {
      return {
        type: 'value',
        value: subExpr
      };
    }
    
    // Находим оператор с наименьшей глубиной вложенности
    let depth = 0;
    let operatorPos = -1;
    let operator = null;
    
    for (let i = 0; i < subExpr.length; i++) {
      const char = subExpr[i];
      
      if (char === '(') depth++;
      else if (char === ')') depth--;
      else if (depth === 0 && (subExpr.substr(i, 2) === '&&' || subExpr.substr(i, 2) === '||')) {
        operatorPos = i;
        operator = subExpr.substr(i, 2);
        break;
      }
    }
    
    // Если оператор найден
    if (operatorPos !== -1) {
      const left = subExpr.substr(0, operatorPos);
      const right = subExpr.substr(operatorPos + 2);
      
      return {
        type: 'operator',
        operator: operator,
        left: parse(left),
        right: parse(right)
      };
    }
    
    // Если выражение в скобках, убираем их и парсим заново
    if (subExpr[0] === '(' && subExpr[subExpr.length - 1] === ')') {
      return parse(subExpr.substring(1, subExpr.length - 1));
    }
    
    // Если ничего не распарсилось, возвращаем пустое значение
    return {
      type: 'value',
      value: ''
    };
  };
  
  return parse(expr);
};

```


```import React, { useState } from 'react';

const LogicBuilder = () => {
  const [expression, setExpression] = useState({
    type: 'operator',
    operator: '&&',
    left: { type: 'value', value: '' },
    right: { type: 'value', value: '' },
  });

  const valueOptions = [
    { label: 'Выберите значение', value: '' },
    { label: 'Значение 1', value: 'value1' },
    { label: 'Значение 2', value: 'value2' },
    { label: 'Значение 3', value: 'value3' },
    { label: 'Другое', value: 'other' },
  ];

  const operatorOptions = [
    { label: 'И (&&)', value: '&&' },
    { label: 'ИЛИ (||)', value: '||' },
  ];

  const handleOperatorChange = (node, newOperator) => {
    if (node.type !== 'operator') return;

    const updateNode = (currentNode) => {
      if (currentNode === node) {
        return { ...currentNode, operator: newOperator };
      }

      if (currentNode.type === 'operator') {
        return {
          ...currentNode,
          left: updateNode(currentNode.left),
          right: updateNode(currentNode.right),
        };
      }

      return currentNode;
    };

    setExpression(updateNode(expression));
  };

  const handleValueChange = (node, newValue) => {
    if (node.type !== 'value') return;

    const updateNode = (currentNode) => {
      if (currentNode === node) {
        return { ...currentNode, value: newValue };
      }

      if (currentNode.type === 'operator') {
        return {
          ...currentNode,
          left: updateNode(currentNode.left),
          right: updateNode(currentNode.right),
        };
      }

      return currentNode;
    };

    setExpression(updateNode(expression));
  };

  const convertToOperator = (node) => {
    if (node.type !== 'value') return;

    const updateNode = (currentNode) => {
      if (currentNode === node) {
        return {
          type: 'operator',
          operator: '&&',
          left: { type: 'value', value: node.value },
          right: { type: 'value', value: '' },
        };
      }

      if (currentNode.type === 'operator') {
        return {
          ...currentNode,
          left: updateNode(currentNode.left),
          right: updateNode(currentNode.right),
        };
      }

      return currentNode;
    };

    setExpression(updateNode(expression));
  };

  const deleteNode = (nodeToDelete, parentNode, isLeftChild) => {
    // Если пытаемся удалить корневой узел, сбрасываем его к начальному состоянию
    if (nodeToDelete === expression) {
      setExpression({
        type: 'operator',
        operator: '&&',
        left: { type: 'value', value: '' },
        right: { type: 'value', value: '' },
      });
      return;
    }

    const updateNode = (currentNode) => {
      // Если текущий узел - родитель узла для удаления
      if (currentNode === parentNode) {
        // Если удаляем левый дочерний узел, заменяем его на пустое значение
        if (isLeftChild) {
          return {
            ...currentNode,
            left: { type: 'value', value: '' },
          };
        }
        // Если удаляем правый дочерний узел, заменяем его на пустое значение
        else {
          return {
            ...currentNode,
            right: { type: 'value', value: '' },
          };
        }
      }

      // Продолжаем рекурсивный обход
      if (currentNode.type === 'operator') {
        return {
          ...currentNode,
          left: updateNode(currentNode.left),
          right: updateNode(currentNode.right),
        };
      }

      return currentNode;
    };

    setExpression(updateNode(expression));
  };

  const renderExpression = (
    node,
    depth = 0,
    parentNode = null,
    isLeftChild = false
  ) => {
    if (node.type === 'value') {
      return (
        <div
          style={{
            marginLeft: `${depth * 20}px`,
            marginBottom: '10px',
            display: 'flex',
            alignItems: 'center',
          }}
        >
          <select
            value={node.value}
            onChange={(e) => handleValueChange(node, e.target.value)}
            style={{ marginRight: '10px' }}
          >
            {valueOptions.map((option) => (
              <option key={option.value} value={option.value}>
                {option.label}
              </option>
            ))}
          </select>
          <button
            onClick={() => convertToOperator(node)}
            style={{ marginRight: '5px', padding: '2px 5px' }}
          >
            +
          </button>
          {parentNode && (
            <button
              onClick={() => deleteNode(node, parentNode, isLeftChild)}
              style={{
                padding: '2px 5px',
                background: '#ff6b6b',
                color: 'white',
                border: 'none',
              }}
            >
              ×
            </button>
          )}
        </div>
      );
    }

    if (node.type === 'operator') {
      return (
        <div style={{ marginLeft: `${depth * 20}px`, marginBottom: '10px' }}>
          <div
            style={{
              display: 'flex',
              alignItems: 'center',
              marginBottom: '5px',
            }}
          >
            <select
              value={node.operator}
              onChange={(e) => handleOperatorChange(node, e.target.value)}
              style={{ marginRight: '10px' }}
            >
              {operatorOptions.map((option) => (
                <option key={option.value} value={option.value}>
                  {option.label}
                </option>
              ))}
            </select>
            {parentNode && (
              <button
                onClick={() => deleteNode(node, parentNode, isLeftChild)}
                style={{
                  padding: '2px 5px',
                  background: '#ff6b6b',
                  color: 'white',
                  border: 'none',
                }}
              >
                ×
              </button>
            )}
          </div>
          <div style={{ borderLeft: '1px solid #ccc', paddingLeft: '10px' }}>
            {renderExpression(node.left, depth + 1, node, true)}
            {renderExpression(node.right, depth + 1, node, false)}
          </div>
        </div>
      );
    }

    return null;
  };

  const formatExpression = (node) => {
    if (node.type === 'value') {
      return node.value || '?';
    }

    if (node.type === 'operator') {
      return `(${formatExpression(node.left)} ${
        node.operator
      } ${formatExpression(node.right)})`;
    }

    return '';
  };

  return (
    <div style={{ fontFamily: 'Arial, sans-serif', padding: '20px' }}>
      <h2>Конструктор логического выражения</h2>
      <div style={{ marginBottom: '20px' }}>{renderExpression(expression)}</div>
      <div
        style={{
          backgroundColor: '#f5f5f5',
          padding: '10px',
          borderRadius: '4px',
        }}
      >
        <strong>Текущее выражение:</strong> {formatExpression(expression)}
      </div>
    </div>
  );
};

export default LogicBuilder;```
