```import React, { useState } from 'react';

const LogicBuilder = () => {
  const [expression, setExpression] = useState({
    type: 'operator',
    operator: '&&',
    left: { type: 'value', value: '' },
    right: { type: 'value', value: '' }
  });

  const valueOptions = [
    { label: 'Выберите значение', value: '' },
    { label: 'Значение 1', value: 'value1' },
    { label: 'Значение 2', value: 'value2' },
    { label: 'Значение 3', value: 'value3' },
    { label: 'Другое', value: 'other' }
  ];

  const operatorOptions = [
    { label: 'И (&&)', value: '&&' },
    { label: 'ИЛИ (||)', value: '||' }
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
          right: updateNode(currentNode.right)
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
          right: updateNode(currentNode.right)
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
          right: { type: 'value', value: '' }
        };
      }
      
      if (currentNode.type === 'operator') {
        return {
          ...currentNode,
          left: updateNode(currentNode.left),
          right: updateNode(currentNode.right)
        };
      }
      
      return currentNode;
    };

    setExpression(updateNode(expression));
  };

  const renderExpression = (node, depth = 0) => {
    if (node.type === 'value') {
      return (
        <div style={{ marginLeft: `${depth * 20}px`, marginBottom: '10px' }}>
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
          <button onClick={() => convertToOperator(node)}>+</button>
        </div>
      );
    }

    if (node.type === 'operator') {
      return (
        <div style={{ marginLeft: `${depth * 20}px`, marginBottom: '10px' }}>
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
          <div style={{ borderLeft: '1px solid #ccc', paddingLeft: '10px' }}>
            {renderExpression(node.left, depth + 1)}
            {renderExpression(node.right, depth + 1)}
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
      return `(${formatExpression(node.left)} ${node.operator} ${formatExpression(node.right)})`;
    }
    
    return '';
  };

  return (
    <div style={{ fontFamily: 'Arial, sans-serif', padding: '20px' }}>
      <h2>Конструктор логического выражения</h2>
      <div style={{ marginBottom: '20px' }}>
        {renderExpression(expression)}
      </div>
      <div style={{ backgroundColor: '#f5f5f5', padding: '10px', borderRadius: '4px' }}>
        <strong>Текущее выражение:</strong> {formatExpression(expression)}
      </div>
    </div>
  );
};

export default LogicBuilder;```
